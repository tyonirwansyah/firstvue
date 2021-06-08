<template>
  <div class="list-container">
    <div class="left-container">
      <h3>{{ list.text }}</h3>
      <p>{{ list.day }}</p>
    </div>
    <div class="right-container">
      <FontAwesomeIcon
        icon="pencil-alt"
        size="lg"
        :color="editHover"
        @click="$emit('editTask', list.id)"
        @mouseenter="editHover = [`#FFD166`]"
        @mouseleave="editHover = [`white`]"
      />
      <FontAwesomeIcon
        icon="trash"
        size="lg"
        :color="trashHover"
        @click="$emit('deleteTask', list.id)"
        @mouseenter="trashHover = [`#EE6352`]"
        @mouseleave="trashHover = [`white`]"
      />
    </div>
  </div>
</template>

<script lang="ts">
import {
  faTrash,
  faPencilAlt,
  faCheck,
} from "@fortawesome/free-solid-svg-icons";
import { defineComponent } from "vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { library } from "@fortawesome/fontawesome-svg-core";

library.add(faTrash, faPencilAlt, faCheck);

export default defineComponent({
  name: "List",
  components: {
    FontAwesomeIcon,
  },
  emits: ["deleteTask", "editTask"],
  props: {
    list: Object,
  },
  data() {
    return {
      trashHover: "white",
      editHover: "white",
    };
  },
});
</script>

<style scoped>
.list-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 2em;
  margin: 12px 12px 12px;
  color: #fff;
  border-radius: 10px;
  background: #095256;
}
.right-container {
  display: flex;
  align-items: center;
  gap: 20px;
  cursor: pointer;
}
</style>
