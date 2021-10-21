<template>
  <div class="window">
    <div class="title">
      <span>任务备忘录：</span>
      <input
        type="text"
        class="inputContent"
        @change="getInputValue"
        ref="inputContent"
        placeholder="在此添加任务"
        @keydown.enter="handleClick"
      />
      <div class="add add-left" @click="handleClick" >添加任务</div>
    </div>
    <List :list="list" />

    
    <div class="add add-right" @click="handleClickRemove">清除所有</div>
  </div>
</template>

<script setup>
import { defineProps, reactive, ref } from "vue";
import List from "./List.vue";
const inputContent = ref(null);
const myStorage = window.localStorage;
let result = JSON.parse(myStorage.getItem("list"));

let isExit = null;

if (result == null) {
  isExit = [];
} else {
  isExit = result;
}
const list = reactive(isExit);

let targetElement = null;
// 获取input值
const getInputValue = (e) => {
  targetElement = e;
};
// 点击添加后清空输入框
const handleClick = () => {
  if (targetElement != null && targetElement.target.value != "") {
    const newTask = {
      content: targetElement.target.value,
      state: false,
      btnText: "未完成",
    };
    list.push(newTask);
    myStorage.setItem("list", JSON.stringify(list));

    targetElement.target.value = "";
  }
};
// 删除所有任务
const handleClickRemove = () => {
  result = JSON.parse(myStorage.getItem("list"));
  if(result!=null){
    list.splice(0,list.length)
    myStorage.removeItem("list")
  }else{
    alert("所有任务已删除")
  }
  console.log("removeResult:",result);
  console.log("removeList:",list);
  
};
</script>

<style scoped lang="scss">
a {
  color: #42b983;
}
.window {
  width: 60vw;
  height: 60vh;
  border: 1px solid white;
  color: white;
  position: relative;
  border: 1px solid #333;
  .title {
    width: 100%;
    height: 6vh;
    padding: 1vh;
    position: sticky;
    top: 0;
    margin: 0;
    background: #333;
    overflow: hidden;

    display: flex;
    justify-content: space-between;
    // align-items: center;
    // flex-wrap: wrap;

    color: white;
    border-bottom: 1px solid white;
    box-sizing: border-box;
    span {
      font-weight: bold;
      font-size: 20px;
    }
  }
  .inputContent {
    // width: 45vw;
    padding-left: 5px;
    flex: 1;
    outline: none;
    height: 3vh;
    // position: absolute;
  }
  .add {
    position: absolute;
    background: #333;
    box-sizing: border-box;
    border: 1px solid white;
    display: inline-block;
    padding: 5px 20px;
    border-radius: 15px;
    bottom: 10px;
    cursor: pointer;
  }
  .add-left {
    position: static;
    margin-left: 15px;
  }
  .add-right {
    right: 20px;
  }
}
</style>
