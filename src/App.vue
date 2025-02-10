<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <div style="position: fixed">
      <div style="display: flex; justify-content: center; align-items: center">
        <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="20" height="20" />
        <span>Instagram</span>
      </div>

      <div class="wrapper">
        <nav style="display: flex; flex-direction: column; flex: 1">
          <RouterLink to="/">首页</RouterLink>
          <RouterLink to="/">搜索</RouterLink>
          <RouterLink to="/">发现</RouterLink>
          <RouterLink to="/">消息</RouterLink>
        </nav>
      </div>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
  height: 100vh;
  display: flex;
  flex-direction: column;
  /* background-color: red; */
  width: 15rem;
  padding: 1rem;
}

.logo {
  display: block;
  /* margin: 0 auto 2rem; */
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    /* margin: 0 2rem 0 0; */
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
