<script lang="ts" setup>
import {ref} from 'vue'
import Task from './Task.vue'

defineProps<{ msg: string }>()
let count = ref(0)
let tasks = ref([{id: 1, name: "Eat muffin", type: 1}])
let typeSort = ref("-1")

const countTasks = () => {
  count.value = tasks.value.length
}

countTasks()

const addTask = () => {
  let myDiv = document.getElementById("task") as HTMLInputElement
  if (myDiv.value === "")
    return
  let newId: number = getMaxId() + 1
  tasks.value.push({id: newId, name: myDiv.value, type: 1})
  myDiv.value = ''
  countTasks()
}

const deleteTask = (id: number) => {
  tasks.value.forEach((task, i) => {
    if (task.id == id)
      tasks.value.splice(i, 1)
  })
  countTasks()
}

const getMaxId = (): number => {
  return Math.max(...tasks.value.map(task => task.id)) == -Infinity ? 0 : Math.max(...tasks.value.map(task => task.id))
}

const doTaskSuccess = (id: number): void => {
  tasks.value.forEach((task) => {
    if (task.id == id)
      task.type = 2
  })
  console.log(tasks)
}

</script>

<template>
  <h1>{{ msg }}</h1>
  <div v-for="task in tasks">
    <template v-if="task.type === 1 && (typeSort ==='-1' || typeSort ==='1')">
      <p class="task task-progress">{{ task.name }} : </p>
      <button class="btn btn-success" @click="doTaskSuccess(task.id)">V</button>
      <button class="btn btn-danger" @click="deleteTask(task.id)">X</button>
    </template>
    <template v-if="task.type === 2 && (typeSort ==='-1' || typeSort ==='2')">
      <p class="task task-finished">{{ task.name }} : </p>
      <button class="btn btn-danger" @click="deleteTask(task.id)">X</button>
    </template>
    <template v-if="task.type === 0 && (typeSort ==='-1' || typeSort ==='0')">
      <p class="task task-stopped">{{ task.name }} : </p>
      <button class="btn btn-danger" @click="deleteTask(task.id)">X</button>
    </template>
  </div>
  <input id="task" name="task" type="text">
  <button class="btn btn-primary" type="button" @click="addTask()">Add</button>
  <button class="btn btn-primary" type="button">count is: {{ count }}</button>

  <select id="sort-task" name="sort" @change="typeSort = $event.target.value">
    <option selected value="-1">All type</option>
    <option value="1">In progress</option>
    <option value="2">Finish</option>
    <option value="0">Stop</option>
  </select>
  <br>
  <br>
  <h1>Test call parents method</h1>
  <div v-for="task in tasks">
    <Task v-bind:idTask=task.id v-bind:nameTask=task.name v-bind:typeTask=task.type
          @deleteTask="(value) => deleteTask(value)" @successTask="(value) => doTaskSuccess(value)"></Task>
  </div>


</template>

<style>
.task {
  display: inline
}

.task-finished {
  color: green;
}

.task-progress {
  color: orange;
}

.task-stopped {
  color: red;
}


button {
  display: inline
}

a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}

p {
  display: inline
}
</style>
