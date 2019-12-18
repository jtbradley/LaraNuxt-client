<template>
  <div class="container col-md-6 mt-5">
    <h2>Create a new topic</h2>
    <br>
    <b-form @submit.prevent="onSubmit">
      <b-form-group label="Topic title:"
        label-for="title"
        description="Title your topic.">
        <b-form-input
          id="title"
          v-model.trim="form.title"
          type="text"

          placeholder="Enter topic title"
          autofocus
        ></b-form-input>
        <small class="text-danger form-text" v-if="errors.title">{{errors.title[0]}}</small>
      </b-form-group>

      <b-form-group label="Body:"
        label-for="body"
        description="Write something interesting.">
        <b-form-textarea
      id="body"
      v-model="form.body"

      placeholder="Enter something for body..."
      rows="3"
      max-rows="6"
    ></b-form-textarea>
        <small class="text-danger form-text" v-if="errors.body">{{errors.body[0]}}</small>
      </b-form-group>

      <b-button type="submit" variant="primary">Create</b-button>
    </b-form>
    <br>
  </div>
</template>

<script>
export default {
  middleware: ['auth'],
  data() {
    return {
      form: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    async onSubmit() {
      await this.$axios.$post('/topics', this.form)
      this.$router.push('/');
    }
  }
}
</script>
