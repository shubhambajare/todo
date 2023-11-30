<template>
    <div style="margin: 10px 20px">

        <el-dialog :model-value="true" title="Create Todo" :width="dialogWidth" @closed="close">
            <div>
                <h3>Title<span style="color: red;">*</span></h3>
                <el-input v-model="title" placeholder="Title" />
                <br />
                <h3>Description<span style="color: red;">*</span></h3>
                <el-input v-model="description" type="textarea" placeholder="Description" />
            </div>
            <template #footer>
                <span class="dialog-footer">
                    <el-button @click="close">Cancel</el-button>
                    <el-button type="primary" @click="addTodo">
                        Create
                    </el-button>
                </span>
            </template>
        </el-dialog>
    </div>
</template>
    
<script setup lang="ts">
import { ElMessage } from 'element-plus';
import { computed, ref } from 'vue';

const emits = defineEmits(['add-todo', 'close']);


const title = ref('');
const description = ref('');
const dialogWidth = computed(() => {
    return window.innerWidth < 500 ? "90%" : "50%"
})
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

const close = () => {
    emits('close');
}

</script>
    
<style scoped></style>
    