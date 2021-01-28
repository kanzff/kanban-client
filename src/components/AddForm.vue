<template>
    <div class="row">
        <div class="col-4 offset-4 bg-info rounded" style="margin-top: 20px; margin-bottom: 20px;padding: 30px;">
            <div class="addForm">
                <h2 style="text-shadow: white 1px 1px;">Add New Task</h2><br>
                <form @submit.prevent="createTask">
                    <input class="rounded" type="text" placeholder="Title" v-model="titleAdd"><br><br>
                    <input class="rounded" type="text" placeholder="Description" v-model="descriptionAdd"><br><br>
                    <select class="rounded" v-model="categoryAdd">
                        <option value="backlog">Backlog</option>
                        <option value="todo">Todo</option>
                        <option value="doing">Doing</option>
                        <option value="done">Done</option>
                    </select><br><br><br>
                    <button class="btn btn-light rounded" type="submit" id="add-task-btn">Add</button>
                    <button class="btn btn-danger" style="margin-left: 230px;" @click="changePage('home')">Cancel</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
const baseUrl = 'https://kanban-kanzff.herokuapp.com'
import axios from 'axios'
export default {
    name: "AddForm",
    props: ['changePage', 'fetchTasks'],
    data() {
        return {
            titleAdd: '',
            descriptionAdd: '',
            categoryAdd: ''
        }
    },
    methods: {
        createTask() {
            // console.log(this.titleAdd, this.descriptionAdd, this.categoryAdd)
            // console.log('masuk create')
            axios({
                method: 'POST',
                url: `${baseUrl}/tasks`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    title: this.titleAdd,
                    description: this.descriptionAdd,
                    category: this.categoryAdd
                }
            })
                .then(({data}) => {
                    console.log('masuk then')
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