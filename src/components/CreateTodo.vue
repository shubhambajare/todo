<template>
    <div style="margin: 10px 20px">
        <el-row :gutter="10">
            <el-col :span="6">
                <el-input v-model="title" placeholder="Title" />
            </el-col>
            <el-col :span="16">
                <el-input v-model="description" placeholder="Description" />
            </el-col>
            <el-col :span="1">
                <el-button type="primary" @click="addTodo">Create</el-button>
            </el-col>
        </el-row>
    </div>
</template>
    
<script setup lang="ts">
import { ElMessage } from 'element-plus';
import { ref } from 'vue';

const emits = defineEmits(['add-todo']);

const title = ref('');
const description = ref('');

const addTodo = () => {
    if (title.value === "") {
        ElMessage.error("Title can not be blank.");
        return;
    }
    if (description.value === "") {
        ElMessage.error("Description can not be blank.");
        return;
    }

    emits('add-todo', { title, description });
}
</script>
    
<style scoped></style>
    