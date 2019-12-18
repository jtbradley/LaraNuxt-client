<template>
  <b-navbar toggleable="lg" class="mb-5" type="dark" variant="info">
    <b-navbar-brand href="/">LaraNuxt</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item to="/">Home</b-nav-item>
        <b-nav-item to="/dashboard">Create</b-nav-item>
        <b-nav-item to="/topics">Topics</b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav v-if="!authenticated" class="ml-auto">
        <b-nav-item to="/login">Login</b-nav-item>
        <b-nav-item to="/register">Register</b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav v-if="authenticated" class="ml-auto">
        <b-nav-item>{{ user.name }}</b-nav-item>
        <b-nav-item @click.prevent="logout">Logout</b-nav-item>
      </b-navbar-nav>


    </b-collapse>

  </b-navbar>
</template>

<script>
export default {
  methods: {
    logout() {
      this.$auth.logout();
    }
  }
}
</script>
