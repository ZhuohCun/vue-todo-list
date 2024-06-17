<script setup lang="ts">
import {ref} from "vue";
interface ITaskItem{
  id:number;
  text:string;
  isDone:boolean;
  isEdit:boolean;
}
let todoText = ref('')  //'reactive' can also create reactive objects
let taskList=ref<ITaskItem[]>([]);
let itemText=ref("")
taskList.value=JSON.parse(window.localStorage.getItem('taskList')??"[]"); //anti-stringify pattern:a??b meaning that if a is empty,the output string will be replaced with b
function onAddTask(){
  taskList.value.push(
      {
        id:taskList.value.length+1,  //repetitive issues remain
        text: todoText.value,
        isDone: false,
        isEdit: false
      }
  );
  todoText.value='';
  onSave();
}
function onSave(){
  window.localStorage.setItem('taskList',JSON.stringify(taskList.value)); //key - value(stringify)
}
function onEdit(item:ITaskItem){
  item.isEdit=true;
  itemText.value=item.text;
}
function onEditOK(item:ITaskItem){
  item.isEdit=false;
  item.text=itemText.value;
  onSave();
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
        <template v-if="!item.isEdit">
            <el-checkbox v-model="item.isDone" label="task1" size="large">{{ item.text }}</el-checkbox>
            <div>
               <el-button type="primary" @click="onEdit(item)">Edit</el-button>
               <el-button type="danger">Delete</el-button>
            </div>
        </template>
        <template v-else>

          <el-input v-model="itemText"></el-input>
          <el-button type="primary" @click="onEditOK(item)">Confirm</el-button>
          <el-button type="danger" @click="item.isEdit=false">Cancel</el-button>
        </template>
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
