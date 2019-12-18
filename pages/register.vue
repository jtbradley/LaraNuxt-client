<template>
  <div class="container col-md-6 mt-5">
    <h2>Register</h2>
    <br>
    <b-form @submit.prevent="onSubmit">
      <b-form-group >
        <b-form-input
          v-model.trim="form.name"
          type="text"
          required
          placeholder="Enter full name"
          autofocus
        ></b-form-input>
        <small class="text-danger form-text" v-if="errors.name">{{errors.name[0]}}</small>
      </b-form-group>

      <b-form-group >
        <b-form-input
          v-model.trim="form.email"
          type="email"
          required
          placeholder="Enter email"
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

      <b-button type="submit" variant="primary">Register</b-button>
    </b-form>
    <br>
    <p>Already have an account? <nuxt-link to="/login">Login</nuxt-link></p>
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
          name: '',
          email: '',
          password: '',
        }
      }
    },
    methods: {
      async onSubmit() {
        try {
        await this.$axios.$post('register', this.form)
        await this.$auth.loginWith('local', {
          data: {
            email: this.form.email,
            password: this.form. password
          }
        })
        //redirect
        this.$router.push({
          path: this.$route.query.redirect || "/dashboard"
        });
      } catch(errors) {
        console.log('Register failed');
    }
  }
    }
  }
</script>
