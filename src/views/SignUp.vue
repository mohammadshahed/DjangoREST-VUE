<template>
    <div class="page-sign-up mt-6">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Register Form</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Username</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Email</label>
                        <div class="control">
                            <input type="email" class="input" v-model="email">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Confirm Password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password2">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-primary">Sign Up</button>
                        </div>
                    </div>

                    <hr>

                    🠮 <router-link to="/log-in">Click Here</router-link> to Log In!
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import { toast } from 'bulma-toast'

    export default {
        name: 'SignUp',
        data() {
            return {
                username: '',
                email: '',
                password: '',
                password2: '',
                errors: []
            }
        },
        methods: {
            submitForm() {
                this.errors = []
                if (this.username === '') {
                    this.errors.push('The username is missing')
                }
                if (this.email === '') {
                    this.errors.push('The email is wrong')
                }
                if (this.password === '') {
                    this.errors.push('The password is too short')
                }
                if (this.password !== this.password2) {
                    this.errors.push('The passwords doesn\'t match')
                }
                if (!this.errors.length) {
                    const formData = {
                        username: this.username,
                        email: this.email,
                        password: this.password,
                        password2: this.password2
                    }
                    axios
                        .post("account/api/register/", formData)
                        .then(response => {
                            toast({
                                message: 'Your Account has benn created, please log in!',
                                type: 'is-success',
                                dismissible: true,
                                pauseOnHover: true,
                                duration: 2000,
                                position: 'bottom-right',
                            })
                            this.$router.push('/log-in')
                            console.log("for testing")
                        })
                        .catch(error => {
                            if (error.response) {
                                for (const property in error.response.data) {
                                    this.errors.push(`${property}: ${error.response.data[property]}`)
                                }
                                console.log(JSON.stringify(error.response.data))
                            } else if (error.message) {
                                this.errors.push('Something went wrong. Please try again')
                                
                                console.log(JSON.stringify(error))
                            }
                        })
                }
            }
        }
    }
</script>