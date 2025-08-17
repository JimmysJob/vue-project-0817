<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <RouterLink to="/">Home</RouterLink>
  <RouterLink to="/WeekTwo">Week 2</RouterLink>

  <RouterView />
</template>

<style scoped>
a {
  padding-right: 20px;
  text-decoration: none;
}
</style>
