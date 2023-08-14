<template>
  <div class="popup">hello component</div>
</template>
<script setup>
import { ref, watch } from 'vue';
const props = defineProps(['answerID']);
console.log(props);
watch(props, () => {
  console.log(props);
});
</script>
<style lang="scss">
.popup {
  position: fixed;
  width: 300px;
  height: 500px;
  background: cyan;
  inset: auto;
}
</style>
