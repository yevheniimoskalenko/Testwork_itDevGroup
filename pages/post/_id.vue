<template>
  <div class="post">
    <div class="content">
      <h2>{{ respons.title }}</h2>
      <nuxt-link :to="{ name: 'author', query: { id: respons.userId } }">
        <span>{{ respons.name }}</span>
      </nuxt-link>
      <p>{{ respons.body }}</p>
    </div>
    <div class="comments">
      <h2>Commens: {{ respons.comments.length }}</h2>
      <div
        v-for="comment in respons.comments"
        :key="comment.id"
        class="comment"
      >
        <span>name: {{ comment.name }}</span>
        <p>{{ comment.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import axios from 'axios'
export default {
  async asyncData({ query }) {
    const { id } = query
    try {
      const respons = await axios({
        method: 'get',
        url: `https://jsonplaceholder.typicode.com/posts/${id}`,
      }).then(async (r) => {
        await axios({
          url: `https://jsonplaceholder.typicode.com/users?id=${r.data.userId}`,
          method: 'get',
        }).then((res) => _.assign(r.data, { name: res.data[0].name }))
        await axios({
          method: 'get',
          url: `https://jsonplaceholder.typicode.com/posts/${id}/comments`,
        }).then((comments) => _.assign(r.data, { comments: comments.data }))
        return await r.data
      })
      return { respons }
    } catch (e) {}
  },
}
</script>

<style lang="scss" scoped></style>
