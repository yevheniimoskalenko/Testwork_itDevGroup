<template>
  <div class="post">
    <h2>{{ respons.title }}</h2>
    <span>{{ respons.userId }}</span>
    <p>{{ respons.body }}</p>

    {{ respons }}
  </div>
</template>

<script>
export default {
  async asyncData({ query }) {
    const { id } = query
    try {
      const data = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${id}`
      )
      const respons = await data.json()
      return { respons }
    } catch (e) {}
  },
}
</script>

<style lang="scss" scoped></style>
