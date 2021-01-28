<template>
    <div class="row">
        <div class="col-4 offset-4 bg-info rounded" style="margin-top: 20px; margin-bottom: 20px;padding: 30px;">
            <div class="editForm">
                <h2 style="text-shadow: white 1px 1px;">Edit Task</h2><br>
                <form @submit.prevent="editTask(taskId)">
                    <input class="rounded" type="text" placeholder="Title" v-model="titleEdit"><br><br>
                    <input class="rounded" type="text" placeholder="Description" v-model="descriptionEdit"><br><br>
                    <select class="rounded" v-model="categoryEdit">
                        <option value="backlog">Backlog</option>
                        <option value="todo">Todo</option>
                        <option value="doing">Doing</option>
                        <option value="done">Done</option>
                    </select><br><br><br>
                    <button class="btn btn-light rounded" type="submit" id="edit-task-btn">Edit</button>
                    <button class="btn btn-danger" style="margin-left: 230px;" @click="changePage('home')">Cancel</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from 'axios'
export default {
    name: "EditForm",
    props: ['changePage', 'fetchTasks', 'taskId'],
    data() {
        return {
            titleEdit: '',
            descriptionEdit: '',
            categoryEdit: ''
        }
    },
    methods: {
        editTask(id) {
            console.log(this.taskId)
            axios({
                method: 'PUT',
                url: `${baseUrl}/tasks/${id}`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    title: this.titleEdit,
                    description: this.descriptionEdit,
                    category: this.categoryEdit
                }
            })
            .then(({data}) => {
                console.log('masuk then edit')
                // console.log(data)
                this.fetchTasks()
                this.changePage('home')
            })
            .catch(err => {
                console.log(err)
            })
        }
    }
}
</script>

<style>

</style>