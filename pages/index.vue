<template>
  <div>
    <div class="infinite-list-wrapper" style="overflow: auto">
      <ul
        v-infinite-scroll="load"
        class="list"
        infinite-scroll-disabled="disabled"
      >
        <div v-for="item in respons" :key="item.id" class="list-item">
          <nuxt-link :to="{ name: 'post', query: { id: `${item.id}` } }"
            ><h2>{{ item.title }}</h2></nuxt-link
          >
          <p>{{ item.body }}</p>
        </div>
      </ul>
      <p v-if="loading">Loading...</p>
      <p v-if="noMore">No more</p>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData() {
    try {
      const data = await fetch('https://jsonplaceholder.typicode.com/posts')
      // const user = await fetch('https://jsonplaceholder.typicode.com/posts')
      const respons = await data.json()
      return { respons }
    } catch (e) {}
  },
  data() {
    return {
      count: 10,
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
