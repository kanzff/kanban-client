<template>
    <div class="card mb-3">
        <div class="card-body">
            <h5 class="card-title">{{task.title}}</h5>
            <p class="card-text">{{task.description}}</p>
            <a href="#" @click="moveLeft(category)" v-if="category !== 'backlog'" class="btn-sm btn-primary">&larr;</a>
            <a href="#" @click="changePage('editForm')" class="btn-sm btn-primary">Edit</a>
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
    props: ['task', 'category', 'changePage', 'fetchTasks'],
    methods: {
        destroy() {
            axios({
                methods: 'DELETE',
                url: `${baseUrl}/tasks/${+this.task.id}`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
            .then(({data}) => {
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
                methods: 'PATCH',
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
                methods: 'PATCH',
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
                this.fetchTasks
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