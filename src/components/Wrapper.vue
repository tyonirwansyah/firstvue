<template>
  <div :style="{ padding: padding }">
    <slot></slot>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Wrapper",
  props: {
    padding: {
      type: String,
      default: "2em",
    },
  },
});
</script>
