<template>
  <ul>
    <li v-for="(item,index) in list" :key="item" :class="{ liStyle: item.state }">
      <div class="priority">
        <div class="op" @click="handlePriority('up',index)">↑</div>
        <div class="op" @click="handlePriority('down',index)">↓</div>
      </div>
      <span class="text">{{ item.content }}</span>
      <div class="btn" :class="{btnInvert: item.state}" @click="handleDeleteList(index)">删除</div>
      <div class="btn" :class="{btnInvert: item.state}" @click="handleStatus(item)">{{ item.btnText }}</div>
      
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";
const props = defineProps({
  list: Array,
});

const handleStatus = (item) => {
  const result = JSON.parse(window.localStorage.getItem("list"));
  console.log(result);
  result.map((task) => {
    if (task.content == item.content) {
      task.state = !task.state;

      if(task.state){
        task.btnText="已完成"
        item.btnText="已完成"
        item.state=true
      }else{
        task.btnText="未完成"
        item.btnText="未完成"
        item.state=false
      }
    }
  });

  window.localStorage.setItem("list", JSON.stringify(result));
};
const handlePriority=(dir='up',index)=>{
  if(dir=='up'&&index==0)return;
  const deletedItem=props.list.splice(index,1)
  if(dir=='up'){
    props.list.splice(--index,0,deletedItem[0])
  }else if(dir=='down'){
    props.list.splice(++index,0,deletedItem[0])
  }
  window.localStorage.setItem("list", JSON.stringify(props.list));
}
const handleDeleteList=(index)=>{
  console.log('当前要删除的元素index为：',index);
  props.list.splice(index,1)
  window.localStorage.setItem("list", JSON.stringify(props.list));
  
}
</script>

<style lang="scss" scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  height: 90%;
  overflow-y: scroll;
}
li {
  color: #333;
  list-style: none;
  border: 1px solid white;
  border: 1px solid #333;
  margin: 1vh 0;
  padding: 1vw;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .text{
    flex: 1;
  }
}
.liStyle {
  background: #333;
  color: white;
  .text {
    text-decoration: line-through;
  }
}
li:last-child {
  margin-bottom: 6vh;
}
.priority{
  margin-right: 10px;
  cursor: pointer;
  .op{
    width: 30px;
    border: 1px solid rgb(105, 105, 105);
    text-align: center;
    border-radius: 5px;
  }
}
.btn {
  width: 80px;
  border: 1px solid #333;
  cursor: pointer;
  display: flex;
  justify-content: center;
  padding: 5px;
  border-radius: 15px;
  margin: 0 5px;
}
.btnInvert{
  border: 1px solid white;
  color: white;
}
</style>