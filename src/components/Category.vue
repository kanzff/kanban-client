<template>
    <div class="col-md-3">
        <div class="card-category bg-light">
            <div :class="recolor">{{category.toUpperCase()}}</div>
            <div class="card-body">
                <task v-for="task in filterTasks" :key="task.id" :task="task" :category="category" :changePage="changePage" :fetchTasks="fetchTasks" ></task>
            </div>
            <div class="card-footer">
                <button type="button" class="btn btn-info" @click="changePage('addForm')">Add Task</button>
            </div>
        </div>
    </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from 'axios'
import Task from "./Task"
export default {
    name:"Category",
    props: ['category', 'tasks', 'changePage', 'fetchTasks'],
    data() {
        return {
            color: ['card-header bg-danger', 'card-header bg-warning', 'card-header bg-success', 'card-header bg-primary']
        }
    },
    components: {
        Task
    },
    computed: {
        recolor() {
            let color = ''
            if (this.category == 'backlog') {
                color = this.color[0]
            } else if (this.category == 'todo') {
                color = this.color[1]
            } else if (this.category == 'doing') {
                color = this.color[2]
            } else {
                color = this.color[3]
            }
            return color
        },
        filterTasks() {
            return this.tasks.filter(el => el.category == this.category)
        }
    }

}
</script>

<style>

</style>