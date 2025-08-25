<script setup>
import { RouterLink, RouterView } from 'vue-router';
</script>

<template>
  <div class="container">
    <header>
      <div class="wrapper">
        <nav>
          <RouterLink to="/">Home</RouterLink>
          <RouterLink to="/WeekTwo">Week 2</RouterLink>
          <RouterLink to="/WeekThree">Week 3</RouterLink>
        </nav>
      </div>
    </header>
  </div>

  <RouterView />
</template>

<style scoped>
a {
  padding-right: 20px;
  text-decoration: none;
}
</style>
