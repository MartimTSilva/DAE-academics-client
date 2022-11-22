<template>
    <b-container>
        <div>
            <h1>Create a new Student</h1>
            <form @submit.prevent="create" :disabled="!isFormValid">
                <b-form-group id="username" description="The username is required" label="Enter your username"
                    label-for="username" :invalid-feedback="invalidUsernameFeedback" :state="isUsernameValid">
                    <b-input id="username" v-model.trim="username" :state="isUsernameValid" trim></b-input>
                </b-form-group>

                <b-form-group id="password" description="The password is required" label="Enter your password"
                    label-for="password" :invalid-feedback="invalidPasswordFeedback" :state="isPasswordValid">
                    <b-input id="password" v-model.trim="password" :state="isPasswordValid" trim></b-input>
                </b-form-group>

                <b-form-group id="name" description="The name is required" label="Enter your name"
                    label-for="name" :invalid-feedback="invalidNameFeedback" :state="isNameValid">
                    <b-input id="name" v-model.trim="name" :state="isNameValid" trim></b-input>
                </b-form-group>

                <b-form-group id="email" description="The e-mail is required" label="Enter your e-mail"
                    label-for="email" :state="isEmailValid">
                    <b-input ref="email" v-model.trim="email" type="email" :state="isEmailValid" required
                    pattern=".+@my.ipleiria.pt" placeholder="Enter your e-mail" />
                </b-form-group>

                <b-select v-model="courseCode" :options="courses" :state="isCourseValid" required value-field="code"
                    text-field="name" class="mb-3">
                    <template v-slot:first>
                        <option :value="null" disabled>-- Please select the Course --
                        </option>
                    </template>
                </b-select>
                <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
                <nuxt-link to="/students">Return</nuxt-link>
                <button type="reset" @click="reset">RESET</button>
                <button @click.prevent="create" :disabled="!isFormValid">CREATE</button>
            </form>
        </div>
    </b-container>
</template>
   

<script>
export default {
    data() {
        return {
            username: null,
            password: null,
            name: null,
            email: null,
            courseCode: null,
            courses: [],
            errorMsg: false
        }
    },

    created() {
        this.$axios.$get('/api/courses').then((courses) => {
            this.courses = courses
        })
    },

    methods: {
        reset() {
            this.errorMsg = false
        },

        create() {
            this.$axios.$post('/api/students', {
                username: this.username,
                password: this.password,
                name: this.name,
                email: this.email,
                courseCode: this.courseCode
            }).then(() => {
                this.$router.push('/students')
            }).catch(error => {
                this.errorMsg = error.response.data
            })
        }
    },

    computed: {
        invalidUsernameFeedback() {
            if (!this.username) {
                return null
            }

            let usernameLen = this.username.length
            if (usernameLen < 3 || usernameLen > 15) {
                return 'The username must be between [3, 15] characters.'
            }

            return ''
        },

        isUsernameValid() {
            if (this.invalidUsernameFeedback === null) {
                return null
            }

            return this.invalidUsernameFeedback === ''
        },

        invalidPasswordFeedback() {
            if (!this.password) {
                return null
            }

            let passwordLen = this.password.length
            if (passwordLen < 3 || passwordLen > 255) {
                return 'The password must be between [3, 255] characters.'
            }

            return ''
        },

        isPasswordValid() {
            if (this.invalidPasswordFeedback === null) {
                return null
            }

            return this.invalidPasswordFeedback === ''
        },

        invalidNameFeedback() {
            if (!this.name) {
                return null
            }

            let nameLen = this.name.length
            if (nameLen < 3 || nameLen > 25) {
                return 'The name must be between [3, 25] characters.'
            }

            return ''
        },

        isNameValid() {
            if (this.invalidNameFeedback === null) {
                return null
            }

            return this.invalidNameFeedback === ''
        },

        isEmailValid() {
            if (!this.email) {
                return null
            }

            /* asks the component if it’s valid. We don’t need to use a regex for
            the e - mail.The input field already does the job for us, because it is of type
            “email” and validates that the user writes an e - mail that belongs to the domain
            of IPLeiria.*/
            return this.$refs.email.checkValidity()
        },

        isCourseValid() {
            if (!this.courseCode) {
                return null
            }

            return this.courses.some(course => this.courseCode === course.code)
        },

        isFormValid() {
            if (!this.isUsernameValid) {
                return false
            }

            if (!this.isPasswordValid) {
                return false
            }

            if (!this.isNameValid) {
                return false
            }

            if (!this.isEmailValid) {
                return false
            }

            if (!this.isCourseValid) {
                return false
            }

            return true
        }
    },
}
</script>