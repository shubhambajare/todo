<template>
  <el-row style="align-items: center; margin: 0 5%;">
    <el-col :span="14">
      <h1 style="color: #337ecc;">
        Todo Manager
      </h1>
    </el-col>
    <el-col :span="10" style="text-align: right;">
      <el-button type="primary" size="large" round @click="handleCreate">Create Todo</el-button>
    </el-col>
  </el-row>
  <CreateTodo v-if="isCreateTodo" :selectedToDo="selectedToDo" @add-todo="(todo) => addTodo(todo)"
    @update-todo="(todo) => updateTodo(todo)" @close="isCreateTodo = false" />
  <ToDoCard v-for="todo in todoList" :todo="todo" @delete-todo="(id) => deleteTodo(id)"
    @toggle-imp="(id) => toggleImp(id)" @edit-todo="(id) => editTodo(id)" />

  <p v-if="todoList.length === 0" style="text-align: center; color: gray;">List is empty. Click on create button to
    create new TODO
    <br />
    <br />
    <el-button type="primary" size="large" round @click="handleCreate">Create Todo</el-button>

  </p>
  <p style="text-align: right; color: gray">Created using Vue3 + Typescript + Element Plus</p>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import ToDoCard from './components/ToDoCard.vue';
import CreateTodo from './components/CreateTodo.vue';
import { ElMessage } from 'element-plus';

onMounted(() => {
  getListFromLocalStorage();
})

const todoList = ref([
  {
    id: 0,
    title: "",
    description: "",
    isImp: false
  }
]);

const selectedToDo = ref({});

const isCreateTodo = ref(false)

const handleCreate = () => {
  selectedToDo.value = {}
  isCreateTodo.value = true
}

const deleteTodo = (id: number) => {
  todoList.value = todoList.value.filter(data => id != data.id)
  updateInLocalStorage();
  ElMessage.success("Todo deleted successfully.")
}

const addTodo = (todo: { title: string, description: string, isImp: false }) => {
  todoList.value.push({ id: todoList.value.length + 1, ...todo })
  isCreateTodo.value = false
  updateInLocalStorage();
  ElMessage.success("Todo created successfully.")
}

const updateTodo = (todo: { id: number, title: string, description: string }) => {
  const tempTodo = todoList.value.find(data => todo.id === data.id)
  if (tempTodo) {
    tempTodo.description = todo.description;
    tempTodo.title = todo.title
  }
  isCreateTodo.value = false
  updateInLocalStorage();
  ElMessage.success("Todo updated successfully.")
}

const editTodo = (id: number) => {
  isCreateTodo.value = true;
  selectedToDo.value = todoList.value.find((todo) => todo.id === id) || {};
}

const toggleImp = (id: number) => {
  todoList.value = todoList.value.map(todo => {
    if (todo.id === id) {
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
