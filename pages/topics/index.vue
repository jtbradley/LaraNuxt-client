<template>
  <div class="container">
    <h2>Latest Topics</h2>
    <div v-for="(topic, index) in topics" :key="index" class="my-5 px-4">
    <h2><nuxt-link :to="{name: 'topics-id', params: {id: topic.id }}">{{ topic.title }}</nuxt-link></h2>

    <div v-if="authenticated">
      <div v-if="user.id === topic.user.id">

        <nuxt-link :to="{name: 'topics-edit', params: {id: topic.id }}">
        <button class="btn border-0 btn-outline-success fa fa-edit float-right"></button></nuxt-link>

        <button @click="deleteTopic(topic.id)" class="btn border-0 font-bold btn-outline-danger fa fa-trash float-right"></button>
        </div>
    </div>

    <p class="text-muted">{{topic.created_at}} by {{topic.user.name}}</p>

    <div v-for="(content, index) in topic.posts" :key="index" class="ml-5 content">
      {{ content.body }}
      <p class="text-muted">{{content.created_at}} by {{content.user.name}}</p>
      <!-- add likes button -->
      <div class="btn btn-outline-primary fa fa-thumbs-up ml-5 mb-2" @click="likePost(topic.id, content)">
        <span class="badge">{{ content.like_count }}</span>
      </div>
    </div>

  </div>
  <b-pagination-nav @change="pageChange" :link-gen="linkGen" :number-of-pages="meta.last_page" base-url="/topics" use-router></b-pagination-nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics : [],
      links: [],
      meta: [],
    }
  },
  async asyncData({$axios, query}) {
    let {data, links, meta} = await $axios.$get(`/topics?page=${query.page}`)
    return {
      topics: data,
      links,
      meta,
    }
  },
  methods: {
    // For regular HREF (or string `to` prop if `use-router` is set)
    linkGen(pageNum) {
      //return `?page=${pageNum}`;
      return pageNum === 1 ? '?' : `?page=${pageNum}`
    },
    async pageChange(key) {
    let {data} = await this.$axios.$get(`/topics?page=${key}`)
    return this.topics = {...this.topics, ...data};
    },
    async deleteTopic(id) {
      await this.$axios.$delete(`/topics/${id}`)
      this.$router.push('/');
    },
    async likePost(topicId, content) {
      const userFromVuex = this.$store.getters["auth/user"];
      if (userFromVuex) {
        // cant like your own post
        if (userFromVuex.id === content.user.id) {
          alert('You cant like your own posts')
        }
        // if user has already liked
        else if (content.users) {
          if (content.users.some(user => user.id === userFromVuex.id)) {
            alert('You have already liked this post')
          } else {
            await this.$axios.$post(`/topics/${topicId}/posts/${content.id}/likes`)
            let {data, links} = await this.$axios.$get(`/topics?page=${this.$route.query.page || 1}`)
            this.topics = data
            this.links = links
          }
        }
      } else {
        alert('Please login')
        this.$router.push('/login')
      }
    }
  },
}
</script>

<style scoped>
  .content {
    padding: 0 10px 0 10px;
  }

</style>
