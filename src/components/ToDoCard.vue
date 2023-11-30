<template>
  <div>
    <el-card shadow="always" class="app-card">
      <el-row>
        <el-col :span="20">
          <h3>
            {{ todo.title }}
          </h3>
          <p>{{ todo.description }}</p>
        </el-col>
        <el-col :span="2" style="text-align: center;">
          <el-icon :size="20" @click="() => toggleImp(todo.title)">
            <StarFilled v-if="todo.isImp" color="#e6a23c" />
            <Star v-else />
          </el-icon>
        </el-col>
        <el-col :span="2" style="text-align: center;">
          <el-button type="primary" :icon="Delete" circle @click="() => deleteTodo(todo)" />
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>
  
<script setup lang="ts">

import { Delete, Star, StarFilled } from '@element-plus/icons-vue'

defineProps(['todo']);
const emits = defineEmits(['delete-todo', 'toggle-imp'])
const deleteTodo = (todo: { title: string, description: string }) => {
  emits('delete-todo', todo)
}

const toggleImp = (title: string) => {
  emits('toggle-imp', title);
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
  