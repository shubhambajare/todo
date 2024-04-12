<template>
  <div>
    <el-card shadow="always" class="app-card">
      <el-row>
        <el-col :xs="18" :sm="21">
          <h3>
            {{ todo.title }}
          </h3>
          <p>{{ todo.description }}</p>
        </el-col>
        <el-col :xs="2" :sm="1" style="text-align: center;">
          <el-icon :size="20" @click="() => toggleImp(todo.id)">
            <StarFilled v-if="todo.isImp" color="#e6a23c" />
            <Star v-else />
          </el-icon>
        </el-col>
        <el-col :xs="2" :sm="1" style="text-align: center;">
          <el-icon>
            <EditPen @click="() => editTodo(todo.id)" />
          </el-icon>
        </el-col>
        <el-col :xs="2" :sm="1" style="text-align: center;">
          <el-button type="primary" :icon="Delete" circle @click="() => deleteTodo(todo.id)" />
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script setup lang="ts">

import { Delete, Star, StarFilled, EditPen } from '@element-plus/icons-vue'

defineProps(['todo']);
const emits = defineEmits(['delete-todo', 'toggle-imp', 'edit-todo'])

const deleteTodo = (id: number) => {
  emits('delete-todo', id)
}

const toggleImp = (id: number) => {
  emits('toggle-imp', id);
}

const editTodo = (id: number) => {
  emits('edit-todo', id);
}

</script>

<style scoped>
.app-card {
  margin: 15px;
}

.app-card .el-row {
  align-items: center;
}
</style>