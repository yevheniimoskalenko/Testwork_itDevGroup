<template>
  <div>
    <ul class="list">
      <div v-for="item in respons" :key="item.id" class="list-item">
        <nuxt-link :to="{ name: 'post-id', query: { id: `${item.id}` } }"
          ><h2>{{ item.title }}</h2></nuxt-link
        >
        <span>{{ item.name }}</span>
        <p>{{ item.body }}</p>
      </div>
    </ul>
  </div>
</template>

<script>
import _ from 'lodash'
import axios from 'axios'
export default {
  async asyncData() {
    try {
      const respons = await axios({
        method: 'get',
        url: 'https://jsonplaceholder.typicode.com/posts',
      }).then(async (r) => {
        await axios({
          method: 'get',
          url: 'https://jsonplaceholder.typicode.com/users',
        }).then(async (res) => {
          const item = []
          for await (const key of r.data) {
            for await (const name of res.data) {
              if (key.userId === name.id) {
                item.push(_.assign(key, { name: name.name }))
              }
            }
          }
          return item
        })
        return r.data
      })
      return { respons }
    } catch (e) {}
  },
  data() {
    return {
      loading: false,
    }
  },
  computed: {
    noMore() {
      return this.count >= 20
    },
    disabled() {
      return this.loading || this.noMore
    },
  },
  methods: {
    load() {
      this.loading = true
      setTimeout(() => {
        this.count += 2
        this.loading = false
      }, 2000)
    },
  },
  head: {
    title: 'test work it dev group',
  },
}
</script>
<style scoped>
.infinite-list-wrapper {
  /* height: 188px; */
}
</style>
