<template>
  <div class="author">
    <div class="posts">
      <div v-for="post in posts" :key="post.id" class="post">
        <h2>{{ post.title }}</h2>
        <p>{{ post.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ query }) {
    const { id } = query
    const posts = await axios({
      url: `https://jsonplaceholder.typicode.com/posts?userId=${id}`,
    }).then((r) => r.data)
    return { posts }
  },
}
</script>

<style lang="scss" scoped></style>
