<template>
    <div style="margin: 10px 20px">

        <el-dialog :model-value="true" :title="selectedToDo?.id ? 'Update Todo' : 'Create Todo'" :width="dialogWidth"
            @closed="close">
            <div>
                <h3>Title<span style="color: red;">*</span></h3>
                <el-input v-model="title" placeholder="Title" />
                <br />
                <h3>Description<span style="color: red;">*</span></h3>
                <el-input v-model="description" :rows="4" type="textarea" row placeholder="Description" />
            </div>
            <template #footer>
                <span class="dialog-footer">
                    <el-button @click="close">Cancel</el-button>
                    <el-button v-if="selectedToDo?.id" type="primary" @click="addTodo">
                        Update
                    </el-button>
                    <el-button v-else type="primary" @click="addTodo">
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

const emits = defineEmits(['add-todo', 'update-todo', 'close']);
const props = defineProps(['selectedToDo'])

const title = ref(props.selectedToDo?.title || '');
const description = ref(props.selectedToDo?.description || '');
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

    if (props.selectedToDo?.id) {
        emits('update-todo', { id: props.selectedToDo.id, title, description });
    } else {
        emits('add-todo', { title, description });
    }
}

const close = () => {
    emits('close');
}

</script>

<style scoped></style>