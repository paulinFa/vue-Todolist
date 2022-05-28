<template>
  <template v-if="typeTask === 1">
    <p class="task task-progress">{{ nameTask }} : </p>
    <button class="btn btn-success" @click="successTask(idTask)">V</button>
  </template>
  <template v-else-if="typeTask === 2">
    <p class="task task-finished">{{ nameTask }} : </p>
  </template>
  <template v-else-if="typeTask === 0">
    <p class="task task-stopped">{{ nameTask }} : </p>
  </template>

  <button class="btn btn-danger" @click="deleteTask(idTask)">X</button>
</template>

<script lang="ts" setup>
defineProps<{ idTask: number, nameTask: string, typeTask: number }>()
const emit = defineEmits(['deleteTask', 'successTask']);

function deleteTask(idTask: number) {
  emit('deleteTask', idTask);
}

function successTask(idTask: number) {
  emit('successTask', idTask);
}
</script>

<style scoped>
button {
  display: inline
}

p {
  display: inline
}
</style>