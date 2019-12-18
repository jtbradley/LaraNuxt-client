<template>
  <div class="container col-md-6 mt-5">
    <h2>Login</h2>
    <br>
    <b-form @submit.prevent="onSubmit">
      <b-form-group >
        <b-form-input
          v-model.trim="form.email"
          type="email"
          required
          placeholder="Enter email"
          autofocus
        ></b-form-input>
        <small class="text-danger form-text" v-if="errors.email">{{errors.email[0]}}</small>
      </b-form-group>

      <b-form-group>
        <b-form-input
          v-model.trim="form.password"
          type="password"
          required
          placeholder="Enter password"
        ></b-form-input>
        <small class="text-danger form-text" v-if="errors.password">{{errors.password[0]}}</small>
      </b-form-group>

      <b-button type="submit" variant="primary">Login</b-button>
    </b-form>
    <br>
    <p>Don't have an account? <nuxt-link to="/register">Register</nuxt-link></p>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card>
  </div>
</template>

<script>
  export default {
    middleware: ['guest'],
    data() {
      return {
        form: {
          email: '',
          password: '',
        }
      }
    },
    methods: {
      async onSubmit() {
        try {
        await this.$auth.loginWith("local", {
          data: this.form
        })
        //this.$router.push('/profile');
        this.$router.push({
          path: this.$route.query.redirect || "/dashboard"
        });
      } catch(errors) {
        // moved to plugins/axios
        //this.$router.push('/login');
      }
    }
  }
  }
</script>
