<script setup lang="ts">
defineProps<{ msg: string }>()
</script>

<template>
  <section class="hello">
    <h2>{{ msg }}</h2>
    <p>
      You’re all set! Edit <code>src/components/HelloMaginarium.vue</code> to start building your game UI.
    </p>
  </section>
</template>

<style scoped>
.hello {
  text-align: center;
  max-width: 720px;
}
code {
  background: #0b1220;
  color: #a5b4fc;
  padding: 0.2rem 0.35rem;
  border-radius: 0.25rem;
}
</style>
