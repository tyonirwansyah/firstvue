<template>
  <a :style="buttonVariant()" :class="[`btn`, size, variant]">
    {{ text }}
  </a>
</template>

<script lang="ts">
import { defineComponent } from "vue";

type buttonSize = "sm" | "md" | "lg";

type buttonVariant = "outline" | "solid";

export default defineComponent({
  name: "Button",
  props: {
    text: String,
    color: {
      type: String,
    },
    size: {
      type: String as () => buttonSize,
      default: "md",
    },
    variant: {
      type: String as () => buttonVariant,
      default: "solid",
    },
  },
  methods: {
    buttonVariant() {
      if (this.variant === "solid") {
        return { backgroundColor: this.color };
      } else if (this.variant === "outline") {
        return {
          color: this.color,
          boxShadow: `0 0 0 3px ${this.color} inset`,
        };
      }
    },
  },
});
</script>

<style scoped>
.btn {
  display: inline-block;
  padding: 12px 24px;
  margin: 10px 10px 10px 0;
  border: 0;
  border-radius: 10px;
  font-family: inherit;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  text-decoration: none;
  user-select: none;
}

.sm {
  padding: 10px 22px;
}
.md {
  padding: 12px 24px;
}
.lg {
  padding: 14px 26px;
}

.outline {
  background: #fff;
  box-shadow: 0 0 0 3px rgba(253, 77, 77) inset;
  color: #fd4d4d;
}

.solid {
  background-color: #fd4d4d;
  color: #fff;
}
</style>
