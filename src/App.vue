<template>
  <CreateTodo @add-todo="(todo) => addTodo(todo)" :key="randomKey" />
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
    description: "this is description for todo 1",
    isImp: false
  },
  {
    title: "Todo 2",
    description: "this is description for todo 2 this is description for todo 2 this is description for todo 2 this is description for todo 2 this is description for todo 2",
    isImp: true

  }
]);

const randomKey = ref(0)

const deleteTodo = (todo: { title: string, description: string }) => {
  todoList.value = todoList.value.filter(data => todo.title != data.title)
}

const addTodo = (todo: { title: string, description: string, isImp: false }) => {
  todoList.value.push(todo)
  randomKey.value++;
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
