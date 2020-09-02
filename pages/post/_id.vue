<template>
  <div class="post">
    <h2>{{ respons.title }}</h2>
    <nuxt-link :to="{ name: 'author', query: { id: respons.userId } }">
      <span>{{ respons.name }}</span>
    </nuxt-link>

    <p>{{ respons.body }}</p>
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
        return await r.data
      })

      return { respons }
    } catch (e) {}
  },
}
</script>

<style lang="scss" scoped></style>
