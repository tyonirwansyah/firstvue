<template>
  <div :key="list.id" v-for="list in lists">
    <List
      :list="list"
      @editTask="$emit('editTask', list.id)"
      @deleteTask="$emit('deleteTask', list.id)"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import List from "./List.vue";

export default defineComponent({
  name: "Lists",
  components: {
    List,
  },
  emits: ["deleteTask", "editTask"],
  props: {
    lists: Array,
  },
});
</script>

<style scoped></style>
