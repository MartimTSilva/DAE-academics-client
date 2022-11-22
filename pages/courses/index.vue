<template>
  <!-- easy components usage, already shipped with bootstrap css-->
  <b-container>
    <!-- try to remove :fields=”fields” to see the magic -->
    <b-table striped over :items="courses" :fields="fields" />
    <nuxt-link to="/" class="btn btn-link">Return</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      fields: ['code', 'name'],
      courses: []
    }
  },

  created() {
    this.$axios.$get('/api/courses').then((courses) => {
      this.courses = courses
      console.log('TESTE: ', courses)
    })
  }
}
</script>