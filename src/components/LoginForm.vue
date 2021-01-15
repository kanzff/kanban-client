<template>
    <div class="row">
        <div class="col-4 offset-4 bg-info rounded" style="margin-top: 20px; margin-bottom: 20px;padding: 30px;">
            <div class="login">
                <h2 style="text-shadow: white 1px 1px;">Login</h2><br>
                <form @submit.prevent="login">
                    <input class="rounded" type="text" placeholder="Email" v-model="email"><br><br>
                    <input class="rounded" type="password" placeholder="Password" v-model="password"><br><br>
                    <button class="btn btn-light rounded" type="submit" id="login-btn">Login</button>
                    <button class="btn btn-primary" style="margin-left: 220px;" @click="changePage('register')">Register</button>
                    <br><br><br>
                    <div class="g-signin2" data-onsuccess="onSignIn" style="margin-left: 0px;"></div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from 'axios'
export default {
    name: "LoginForm",
    data() {
        return {
            email: '',
            password: ''
        }
    },
    props: ['changePage', 'fetchTasks'],
    methods: {
        login() {
            axios({
                method: 'POST',
                url: `${baseUrl}/login`,
                data: {
                    email: this.email,
                    password: this.password
                }
            })
            .then(({data}) => {
                console.log(data)
                localStorage.setItem('access_token', data.access_token)
                this.fetchTasks()
                this.changePage('home')
            })
            .catch(err => {
                console.log(err)
            })
        },
        onSignIn(googleUser) {
            const id_token = googleUser.getAuthResponse().id_token;
            axios({
                method: 'POST',
                url: `${baseUrl}/googleLogin`,
                data: { id_token },
            })
            .then(({data}) => {
                localStorage.setItem('access_token', response.access_token);
                this.changePage('home')
                this.fetchTasks()
            })
            .catch(err => {
                console.log(err)
            });
        }
    }
}
</script>

<style>

</style>