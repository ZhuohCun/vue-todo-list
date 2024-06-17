<script setup lang="ts">
import {ref} from "vue";
let todoText = ref('')  //'reactive' can also create reactive objects
let taskList=ref<{id:number,text:String,isDone:boolean}[]>([]);
taskList.value=JSON.parse(window.localStorage.getItem('taskList')??"[]"); //anti-stringify pattern:a??b meaning that if a is empty,the output string will be replaced with b
function onAddTask(){
  taskList.value.push(
      {
        id:taskList.value.length+1,  //repetitive issues remain
        text: todoText.value,
        isDone: false
      }
  );
  todoText.value='';
  window.localStorage.setItem('taskList',JSON.stringify(taskList.value)); //key - value(stringify)
}
</script>

<template>
  <div class="container">
    <h1>Todo List</h1>
    <div class="input-row">
      <el-input style="width:20em" placeholder="Please enter a todo item." v-model="todoText"/>
      <el-button type="success" @click="onAddTask()">Add</el-button>
    </div>
    <div class="task-list">
      <div v-for="item of taskList" class="task-item">
      <el-checkbox v-model="item.isDone" label="task1" size="large">{{ item.text }}</el-checkbox>
        <div>
           <el-button type="primary">Edit</el-button>
           <el-button type="danger">Delete</el-button>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 80%;
  margin: 0 auto;
  background-color: #f3f3f3;
}
h1{
  text-align: center;
  margin-top: 20px;
}
.input-row{
  display: flex;
  justify-content: center;
  margin-top: 20px;
  gap:0.5em;
}
.task-list{
  padding: 0.2em;
}
.task-item{
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ccc;
}
</style>
