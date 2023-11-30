<template>
  <el-row style="align-items: center; margin: 0 5%;">
    <el-col :span="14">
      <h1 style="color: #337ecc;">
        Todo Manager
      </h1>
    </el-col>
    <el-col :span="10" style="text-align: right;">
      <el-button type="primary" size="large" round @click="isCreateTodo = true">Create Todo</el-button>
    </el-col>
  </el-row>
  <CreateTodo v-if="isCreateTodo" @add-todo="(todo) => addTodo(todo)" @close="isCreateTodo = false" />
  <ToDoCard v-for="todo in todoList" :todo="todo" @delete-todo="(todo) => deleteTodo(todo)"
    @toggle-imp="(title) => toggleImp(title)" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import ToDoCard from './components/ToDoCard.vue';
import CreateTodo from './components/CreateTodo.vue';

const todoList = ref([
  {
    title: "Todo 1",
    description: "Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat",
    isImp: false
  },
  {
    title: "Todo 2",
    description: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam.",
    isImp: true

  }
]);

const isCreateTodo = ref(false)

const deleteTodo = (todo: { title: string, description: string }) => {
  todoList.value = todoList.value.filter(data => todo.title != data.title)
}

const addTodo = (todo: { title: string, description: string, isImp: false }) => {
  todoList.value.push(todo)
  isCreateTodo.value = false
}

const toggleImp = (title: string) => {
  todoList.value = todoList.value.map(todo => {
    if (todo.title === title) {
      todo.isImp = !todo.isImp
    }
    return todo;
  })
}

</script>

<style scoped></style>
