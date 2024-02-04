<template>
  <NuxtPage />
</template>

<script setup lang="ts">
const {query} = useRoute();

useHead({
  htmlAttrs: {
    "data-theme" : query.theme || process.client && window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light"
  }
})
</script>