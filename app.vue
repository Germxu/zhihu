<template>
  <NuxtPage />
</template>

<script setup lang="ts">
const {query} = useRoute();
console.log("query", query);

useHead({
  htmlAttrs: {
    // @ts-ignore
    "data-theme" : query.theme || (process.client && window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light")
  }
})
</script>