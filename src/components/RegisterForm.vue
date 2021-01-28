<template>
    <div class="row">
        <div class="col-4 offset-4 bg-info rounded" style="margin-top: 20px; margin-bottom: 20px;padding: 30px;">
            <div class="register">
                <h2 style="text-shadow: white 1px 1px;">Register</h2><br>
                <form @submit.prevent="register">
                    <input class="rounded" type="text" placeholder="Email" v-model="emailRegister"><br><br>
                    <input class="rounded" type="text" placeholder="Username" v-model="usernameRegister"><br><br>
                    <input class="rounded" type="password" placeholder="Password" v-model="passwordRegister"><br><br>
                    <input class="rounded" type="password" placeholder="Confirm Password" v-model="passwordRegisterConfirm"><br><br>
                    <button class="btn btn-light rounded" type="submit" id="register-btn">Register</button>
                    <button class="btn btn-primary" style="margin-left: 230px;" @click="changePage('login')">Login</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
const baseUrl = 'http://localhost:3000'
import axios from 'axios'
export default {
    name: "RegisterForm",
    data() {
        return {
            emailRegister: '',
            passwordRegister: '',
            passwordRegisterConfirm: '',
            usernameRegister: ''
        }
    },
    props: ['changePage'],
    methods: {
        register() {
            // console.log(this.emailRegister, this.passwordRegister, this.passwordRegisterConfirm)
            if (this.passwordRegister == this.passwordRegisterConfirm) {
                axios({
                    method: 'POST',
                    url: `${baseUrl}/register`,
                    data: {
                        email: this.emailRegister,
                        password: this.passwordRegister,
                        username: this.usernameRegister
                    }
                })
                .then(({data}) => {
                    console.log(data)
                    this.changePage('login')
                })
                .catch(err => {
                    console.log(err)
                })
            } else {
                console.log('pass didnt match')
            }
        }
    }
}
</script>

<style>

</style>