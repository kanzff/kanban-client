<template>
  <div class="container-fluid">
      <div class="row">
          <div class="nav bg-info">
              <h3 style="margin-left: 15px;">KANBAN</h3>
              <button v-if="currentPage == 'home'" class="btn-danger rounded" id="logout-btn" @click="logout">Logout</button>
          </div>
      </div>
      <register-form v-if="currentPage == 'register'" :changePage="changePage"></register-form>
      <login-form v-else-if="currentPage == 'login'" :changePage="changePage" :fetchTasks="fetchTasks"></login-form>
      <add-form v-else-if="currentPage == 'addForm'" :changePage="changePage" :fetchTasks="fetchTasks"></add-form>
      <edit-form v-else-if="currentPage == 'editForm'" :changePage="changePage" :taskId="taskId" :fetchTasks="fetchTasks"></edit-form>
      <home v-else :categories="categories" :tasks="tasks" :insertTaskId="insertTaskId" :changePage="changePage" :fetchTasks="fetchTasks"></home>
  </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from "axios"
import RegisterForm from "./components/RegisterForm";
import LoginForm from "./components/LoginForm";
import Home from "./components/Home";
import AddForm from "./components/AddForm";
import EditForm from "./components/EditForm"

export default {
    name: "App",
    data() {
        return {
            message: 'vue masuk',
            currentPage: 'home',
            categories: ['backlog', 'todo', 'doing', 'done'],
            tasks: [],
            taskId: ''
        }
    },
    components: {
        LoginForm,
        RegisterForm,
        Home,
        AddForm,
        EditForm
    },
    methods: {
        changePage(page) {
            // console.log('masuk cgange page', page)
            this.currentPage = page
        },
        logout() {
            localStorage.clear()
            // var auth2 = gapi.auth2.getAuthInstance();
            // auth2.signOut().then(function () {
            //     console.log('User signed out.');
            // });

            this.changePage('login')
        },
        fetchTasks() {
            axios({
                method: "GET",
                url: `${baseUrl}/tasks`,
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
            .then(({data}) => {
                this.tasks = data
            })
            .catch(err => {
                console.log(err)
            })
        },
        insertTaskId(id) {
            this.taskId = id
        }
    },
    created() {
        if (!localStorage.access_token) {
            this.changePage('login')
        } else {
            this.fetchTasks()
            this.changePage('home')
        }
    }
}
</script>

<style>

</style>