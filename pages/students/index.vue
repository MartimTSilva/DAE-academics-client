<template>
  <!-- easy components usage, already shipped with bootstrap css-->
  <b-container>
    <!-- try to remove :fields=”fields” to see the magic -->
    <b-table striped over :items="students" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link pt-0" :to="`/students/${row.item.username}`">Details</nuxt-link>
        <nuxt-link :to="`/students/${row.item.username}/send-email`">Send email</nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/" class="btn btn-link">Back</nuxt-link>
    <nuxt-link to="/students/createStudent" class="btn btn-link">Create New Student</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      fields: ['username', 'name', 'email', 'courseName', 'actions'],
      students: []
    }
  },

  created() {
    this.$axios.$get('/api/students').then((students) => {
      this.students = students
    })
  }
}
</script>