<template>
    <form @submit.prevent="create" class="p-3">
        <div>
            username: <input v-model="username" type="text">
        </div>
        <div class="pt-2">
            password: <input v-model="password" type="password">
        </div>
        <div class="pt-2">
            name: <input v-model="name" type="text">
        </div>
        <div class="pt-2">
            email: <input v-model="email" type="email">
        </div>
        <div class="pt-2">
            course code:
            <select v-model="courseCode">
                <template v-for="course in courses">
                    <option :key="course.code" :value="course.code">
                        {{ course.name }}
                    </option>
                </template>
            </select>
        </div>
        <br>
        <nuxt-link to="/students">Return</nuxt-link>
        <button type="reset">RESET</button>
        <button @click.prevent="create">CREATE</button>
    </form>
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
            courses: []
        }
    },
    
    created() {
        this.$axios.$get('/api/courses')
            .then(courses => {
                console.log('TESTE: ', courses[0].code)
                this.courses = courses
            })
    },

    methods: {
        create() {
            this.$axios.$post('/api/students', {
                username: this.username,
                password: this.password,
                name: this.name,
                email: this.email,
                courseCode: this.courseCode
            })
                .then(() => {
                    this.$router.push('/students')
                })
        }
    }
}
</script>