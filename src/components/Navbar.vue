<template>
  <nav class="main_nav">
    <router-link :to="{name: 'Home'}">Projects</router-link>
    <router-link :to="{name: 'AddProject'}">
      <ion-icon name="create-outline"></ion-icon>
      Add new Project
    </router-link>
  </nav>
</template>

<script>
export default {
  name: "NavBar"
}
</script>

<style scoped>
.main_nav {
  text-align: center;
  margin: 40px auto;
}

.main_nav a {
  display: inline-block;
  text-decoration: none;
  margin: 0 10px;
  color: #999;
  font-size: 18px;
}

a.router-link-active {
  border-bottom: 2px solid #00ce89;
  padding-bottom: 4px;
}
</style>