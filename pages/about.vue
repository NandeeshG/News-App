<template>
  <v-app>
    <h1>ABOUT ME</h1>
    <NuxtLink to="/"> Home page </NuxtLink>
  </v-app>
</template>

<script>
export default {
  layout: 'empty',
  data() {
    return {}
  },
  head() {
    const title = 'NV | About me'
    return {
      title,
    }
  },
}
</script>

<style scoped>
h1 {
  font-size: 20px;
}
</style>
