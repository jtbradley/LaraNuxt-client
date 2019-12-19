<template>
  <div class="container">

    <div class="my-5 px-4">
    <h2 class="display-3">{{topic.title}}</h2>
    <hr>

    <p class="text-muted">{{topic.created_at}} by {{topic.user.name}}</p>

    <div v-for="(content, index) in topic.posts" :key="index" class="ml-5 content">
      <p>{{ content.body }}</p>

      <div v-if="authenticated">
      <div v-if="user.id === content.user.id">

        <button @click="deletePost(content.id)" class="btn border-0 font-bold btn-outline-danger fa fa-trash float-right"></button>

        <nuxt-link :to="{name: 'topics-posts-edit', params: {id: $route.params.id, body: content.id }}">
        <button class="btn border-0 btn-outline-success fa fa-edit float-right"></button></nuxt-link>


        </div>
    </div>

      <p class="text-muted">{{content.created_at}} by {{content.user.name}}</p>
    </div>

  </div>

  <div class="my-5 ml-5" v-if="authenticated">
    <form @submit.prevent="create">
      <div class="form-group">
        <h4 for="">Add a new post</h4>
        <textarea v-model="body" type="text" class="form-control" placeholder="Write something" rows="5"></textarea>
        <small v-if="errors.body" class="form-text text-danger">{{errors.body[0]}}</small>
      </div>
      <button class="btn btn-outline-primary">Add a new post</button>
    </form>
  </div>

  </div>
</template>

<script>
export default {
  data(){
    return {
      topic: '',
      body: ''
    }
  },
  async asyncData({$axios, params}) {
    const {data} = await $axios.$get(`/topics/${params.id}`)
    return {
      topic: data
    }
  },
  methods: {
    async create() {
      await this.$axios.$post(`/topics/${this.$route.params.id}/posts`, {body: this.body})
      .then(response => {
        this.$router.push('/topics')
      })
      .catch(error => {
        //console.log(error.response)
      })
    },
    async deletePost(id) {
      await this.$axios.$delete(`/topics/${this.$route.params.id}/posts/${id}`)
      .then(response => {
        this.$router.push('/topics')
      })
      .catch(error => {
        //console.log(error.response)
      })
    }
  }
}
</script>

<style scoped>
  .content {
    padding: 0 10px 0 10px;
  }
</style>
