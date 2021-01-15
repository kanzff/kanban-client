<template>
  <div class="container-fluid">
      <div class="row">
          <div class="nav bg-info">
              <h3 style="margin-left: 15px;">KANBAN</h3>
              <button v-if="currentPage == 'home'" class="btn-danger rounded" id="logout-btn" @click="logout">Logout</button>
          </div>
      </div>
      <register-form v-if="currentPage == 'register'" :changePage="changePage"></register-form>
      <login-form v-else-if="currentPage == 'login'" :changePage="changePage"></login-form>
      <home v-else></home>
  </div>
</template>

<script>
import axios from "axios"
import RegisterForm from "./components/RegisterForm";
import LoginForm from "./components/LoginForm";
import Home from "./components/Home"

export default {
    name: "App",
    data() {
        return {
            message: 'vue masuk',
            currentPage: 'login',
        }
    },
    components: {
        LoginForm,
        RegisterForm,
        Home
    },
    methods: {
        changePage(page) {
            // console.log('masuk cgange page', page)
            this.currentPage = page
        },
        logout() {
            localStorage.clear()
            this.changePage('login')
        }
    },
    created() {
        if (localStorage.access_token) {
            this.changePage('login')
        } else {
            this.changePage('home')
        }
    }
}
</script>

<style>

</style>