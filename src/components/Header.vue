<template>
  <h1 class="header">
    {{ text }}
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Header",
  props: {
    text: String,
  },
});
</script>

<style scoped>
.header {
  font-size: 2rem;
}
</style>
