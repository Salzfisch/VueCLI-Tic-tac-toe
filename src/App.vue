<script >
import Cell from "./components/AppCell.vue"  //  导入子组件
export default {
  components: { Cell },  //注册子组件
  data() {
    return { 
      n: 0,  //记录当前是第几手
      map: [  //用二维数组来表示棋盘，并记录棋盘上的状态，null表示空，x表示玩家1，o表示玩家2
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ], 
      result: null,  //记录游戏的结果，null表示未分胜负，x表示玩家1赢，o表示玩家2赢
    };
  },
  methods: {
    onClickCell(i, text) {  //定义一个方法，当点击某个格子时触发
      // console.log("某个Cell被点击了");
      console.log(`${i}号被点击了，内容是：${text}`);  //在控制台打印出被点击的格子的编号和内容
      // i
      // 0 map[0][0]
      // 1 map[0][1]
      // 2 map[0][2]
      // 3 map[1][0]
      // 4 map[1][1]
      // 5 map[1][2]
      // 6 map[2][0]
      // 7 map[2][1]
      // 8 map[2][2]
      // console.log(this)
      this.map[Math.floor(i / 3)][i % 3] = text; //更新棋盘上的状态，根据编号计算出对应的行和列
      this.n += 1;  //增加一手
      this.tell();  //调用tell来判断谁胜谁负
    },
    tell() {  //定义一个方法，用来判断游戏的结果
      const map = this.map;  //获取棋盘上的状态
      for(let i = 0; i < 3; i++) {  //遍历每一行
        if(  //如果第一列不为空，并且和第二列、第三列相同
          map[i][0] !== null &&
          map[i][0] === map[i][1] &&
          map[i][1] === map[i][2]
        ) {
          this.result = map[i][2];  //那么就说明这一行形成了三连，更新游戏结果为这一行的内容
        }
      }
      for(let j = 0; j < 3; j++) {  //遍历每一列
        if(  //如果第一行不为空，并且和第二行、第三行相同
          map[0][j] !== null &&
          map[0][j] === map [1][j] &&
          map[1][j] === map [2][j]
        ) {
          this.result = map[2][j];  //那么就说明这一列形成了三连，更新游戏结果为这一列的内容
        }
      }
      if(  //如果左上角不为空，并且和中间、右下角相同
          map[0][0] !== null &&
          map[0][0] === map [1][1] &&
          map[1][1] === map [2][2]
        ) {
          this.result = map[2][2];  //那么就说明这条对角线形成了三连，更新游戏结果为这条对角线的内容
      }
      if(  //如果右上角不为空，并且和中间、左下角相同
          map[0][2] !== null &&
          map[0][2] === map [1][1] &&
          map[1][1] === map [2][0]
        ) {
          this.result = map[2][0];  //那么就说明这条对角线形成了三连，更新游戏结果为这条对角线的内容
      }
    },
    reset() {  // 点击按钮重置游戏
      this.n = 0;  // 将n重置为0，表示重新开始计数
      this.map = [  // 将map重置为一个空的二维数组，表示清空棋盘「这里只清空父组件的数据，并没有清空子组件里的'x'&'o'」
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ];
      this.result = null;  //将result重置为null，表示没有胜负结果
      for(let i = 0; i < 9; i++) {  // 遍历9个子组件
        this.$refs[`cell${i}`].resetSelf();  // 调用每个子组件的resetSelf方法，重置它们的状态
      }
    }
  },
  computed: {
    isOver() {  // 用来判断游戏是否结束，目的阻止继续下棋
      return this.result !== null || this.n === 9;  //如果有结果或者已经下了9手，就说明游戏结束了
    }
  }
}
</script>

<template>
  <div class="chessBoard">  <!--定义一个div元素，作为棋盘的容器-->
    <div>第{{ n }}手</div>  <!--显示当前是第几手，使用插值表达式{{ n }}-->
    <div class="row">  <!--定义一个div元素，作为第一行的容器-->
      <Cell ref="cell0" v-on:click="onClickCell(0, $event)" v-bind:n="n" v-bind:isOver="isOver"/>  <!--使用子组件Cell，绑定click事件，传递n和isOver作为属性-->
      <Cell ref="cell1" v-on:click="onClickCell(1, $event)" v-bind:n="n" v-bind:isOver="isOver"/>  <!--同上，注意编号和位置的对应关系-->
      <Cell ref="cell2" v-on:click="onClickCell(2, $event)" v-bind:n="n" v-bind:isOver="isOver"/>  <!--同上-->
    </div>
    <div class="row">  <!--定义一个div元素，作为第二行的容器-->
      <Cell ref="cell3" v-on:click="onClickCell(3, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
      <Cell ref="cell4" v-on:click="onClickCell(4, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
      <Cell ref="cell5" v-on:click="onClickCell(5, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
    </div>
    <div class="row">  <!--定义一个div元素，作为第三行的容器-->
      <Cell ref="cell6" v-on:click="onClickCell(6, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
      <Cell ref="cell7" v-on:click="onClickCell(7, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
      <Cell ref="cell8" v-on:click="onClickCell(8, $event)" v-bind:n="n" v-bind:isOver="isOver"/>
    </div>
    <!--<div>{{ map }}</div>-->  <!--用来调试棋盘状态的-->
    <div>{{ result !== null ? `${result} 赢了`: "胜负未分" }}</div>  <!--显示游戏结果，使用三元表达式和模板字符串来动态生成内容-->
    <button @click.stop="reset" onclick="console.clear()">重新开始</button>
  </div>
</template>

<style scoped>  /* 定义样式，使用scoped属性来限制作用域，避免影响其他组件  */
.chessBoard {
  display: flex;
  flex-direction: column;  /* 设置主轴方向为垂直 */
  justify-content: center;  /* 设置主轴对齐方式为居中 */
  align-items: center;  /* 设置交叉轴对齐方式为居中 */
}
.row {
  display: flex;
}
button {
  margin-top: 10px;
}
</style>
