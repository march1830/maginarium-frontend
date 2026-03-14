<template>
  <main class="container">
    <header class="header">
      <img alt="Maginarium" src="/favicon.svg" class="logo" />
      <h1>Maginarium</h1>
      <p class="tagline">Vue 3 + Vite starter, ready for Vercel</p>
    </header>

    <HelloMaginarium msg="Welcome to Maginarium" />

    <section class="links">
      <a href="https://vuejs.org/guide/introduction.html" target="_blank" rel="noreferrer">Vue Docs</a>
      <a href="https://vitejs.dev/guide/" target="_blank" rel="noreferrer">Vite Guide</a>
      <a href="https://vercel.com/docs" target="_blank" rel="noreferrer">Vercel Docs</a>
    </section>
  </main>
</template>

<script setup lang="ts">
import HelloMaginarium from './components/HelloMaginarium.vue'
</script>

<style scoped>
.container {
  display: grid;
  grid-template-rows: auto 1fr auto;
  gap: 2rem;
  min-height: 100vh;
  place-items: center;
  padding: 2rem;
}
.header {
  text-align: center;
}
.logo {
  height: 64px;
}
.tagline {
  color: #64748b;
}
.links {
  display: flex;
  gap: 1rem;
}
.links a {
  color: #42b883;
}
</style>
