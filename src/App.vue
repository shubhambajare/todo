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

  <p v-if="todoList.length === 0" style="text-align: center; color: gray;">List is empty. Click on create button to
    create new TODO
    <br />
    <br />
    <el-button type="primary" size="large" round @click="isCreateTodo = true">Create Todo</el-button>

  </p>
  <p style="text-align: right; color: gray">Created using Vue3 + Typescript + Element Plus</p>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import ToDoCard from './components/ToDoCard.vue';
import CreateTodo from './components/CreateTodo.vue';

onMounted(() => {
  getListFromLocalStorage();
})

const todoList = ref([
  {
    title: "",
    description: "",
    isImp: false
  }
]);

const isCreateTodo = ref(false)

const deleteTodo = (todo: { title: string, description: string }) => {
  todoList.value = todoList.value.filter(data => todo.title != data.title)
  updateInLocalStorage();
}

const addTodo = (todo: { title: string, description: string, isImp: false }) => {
  todoList.value.push(todo)
  isCreateTodo.value = false
  updateInLocalStorage();
}

const toggleImp = (title: string) => {
  todoList.value = todoList.value.map(todo => {
    if (todo.title === title) {
      todo.isImp = !todo.isImp
    }
    return todo;
  })
  updateInLocalStorage();
}

const updateInLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
}

const getListFromLocalStorage = () => {
  todoList.value = JSON.parse(localStorage.getItem("todoList") || "[]")
}

</script>

<style scoped></style>
