<template>
    <div class="card mb-3">
        <div class="card-body">
            <h5 class="card-title">{{task.title}}</h5>
            <p class="card-text">{{task.description}}</p>
            <a href="#" @click="moveLeft(category)" v-if="category !== 'backlog'" class="btn-sm btn-primary">&larr;</a>
            <a href="#" @click="toEditForm(task.id)" class="btn-sm btn-primary">Edit</a>
            <a href="#" @click="destroy" class="btn-sm btn-primary">Delete</a>
            <a href="#" @click="moveRight(category)" v-if="category !== 'done'" class="btn-sm btn-primary">&rarr;</a>
        </div>
    </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from 'axios'
export default {
    name: "Task",
    props: ['task', 'category', 'changePage', 'fetchTasks', 'insertTaskId'],
    data () {
        return {
            taskId: this.task.id
        }
    },
    methods: {
        destroy() {
            axios({
                method: 'DELETE',
                url: `${baseUrl}/tasks/${+this.task.id}`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
            .then(({data}) => {
                this.fetchTasks()
                console.log('task deleted')
            })
            .catch(err => {
                console.log(err)
            })
        },
        moveLeft(category) {
            let newCategory = ''
            if (category == 'todo') {
                newCategory = 'backlog'
            } else if (category == 'doing') {
                newCategory = 'todo'
            } else {
                newCategory = 'doing'
            }
            axios({
                method: 'PATCH',
                url: `${baseUrl}/tasks/${+this.task.id}`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    category: newCategory
                }
            })
            .then(({data}) => {
                console.log(data)
                this.fetchTasks()
                this.changePage('home')
            })
            .catch(err => {
                console.log(err)
            })
        },
        moveRight(category) {
            let newCategory = ''
            if (category == 'backlog') {
                newCategory = 'todo'
            } else if (category == 'todo') {
                newCategory = 'doing'
            } else {
                newCategory = 'done'
            }
            axios({
                method: 'PATCH',
                url: `${baseUrl}/tasks/${+this.task.id}`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    category: newCategory
                }
            })
            .then(({data}) => {
                console.log(data)
                this.fetchTasks()
                this.changePage('home')
            })
            .catch(err => {
                console.log(err)
            })
        },
        toEditForm(id) {
            this.insertTaskId(id)
            this.changePage('editForm')
        }
    }
}
</script>

<style>

</style>