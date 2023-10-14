<script>
export default {
  props: ["n", "isOver"],  //定义两个属性，分别接收父组件传递的n和isOver
  data() {
    // return { a: false, text: "x"};
    // return { a: false};
    return { a: false, text: ""};  //定义两个数据，a表示是否被点击过，text表示显示的内容
  },
  methods: {
    onClickSelf() {  //定义一个方法，当自己被点击时触发
        if(this.isOver) { //如果游戏结束，就不执行下面的代码
          return;
        }
        if(this.text !== ""){  //如果已经有内容了，就不执行下面的代码
          return;
        }
        this.a = true;  //设置a为true，表示被点击过了
        // this.a.color = yellow;
        this.text = this.n % 2 ? "x" : "o";  //根据n的奇偶性，设置text为x或o
        this.$emit("click", this.text);  //向父组件发送一个click事件，并传递text作为参数
    },
    resetSelf() {
      this.a = false;  // 将a重置为false，表示取消显示内容
      this.text = "";  // 将text重置为空字符串，表示清空内容
    }
  }
}
</script>

<template> 
  <div>
    <!-- {{ n }} -->
    <div class="cell" v-bind:class="{x:text === 'x'}" v-on:click.stop="onClickSelf">  <!--定义一个div元素，作为格子的容器，绑定class和click事件，并阻止事件冒泡-->
      <template v-if="a">{{ text }}</template>
      <template v-else></template>
    </div>
  </div>
</template>

<style>
/* scoped */
.cell {
  border: 1px solid black;
  width: 100px;
  height: 100px;
  /* display: flex;
  justify-content: center;
  align-items: center; */
  color: red;
  font-size: 80px;
  cursor: pointer;
  text-align: center;
  line-height: 100%;
  user-select: none; /* 阻止浏览器默认行为「选择文本」 */
  -webkit-tap-highlight-color: transparent;  /* 阻止手机浏览器默认行为「点击时给元素高亮的效果」 */
}
.cell.x {
  color: black;
}
</style>

