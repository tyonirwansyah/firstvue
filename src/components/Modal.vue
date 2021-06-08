<template>
  <div class="modal-backdrop">
    <form>
      <div class="modal">
        <h2 class="task-label" style="text-align: left; margin-bottom: 20px">
          {{ title }}
        </h2>
        <p style="text-align: left; margin-bottom: 12px">Your Task</p>
        <input
          class="input"
          name="task"
          type="text"
          placeholder="Type your task"
          style="margin-bottom: 1em"
          autocomplete="off"
          v-model="task"
        />
        <p style="text-align: left; margin-bottom: 12px">
          Due Date
        </p>
        <input
          class="input"
          name="date"
          type="text"
          placeholder="Day & Time"
          style="margin-bottom: 1.5em"
          autocomplete="off"
          v-model="dueDate"
        />
        <div style="display: flex">
          <Button
            text="Close"
            @click="close"
            size="sm"
            style="margin-bottom:0;"
          />
          <Button
            text="Submit"
            @click="handleSubmit"
            size="sm"
            variant="outline"
            style="margin-bottom:0;"
          />
        </div>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "./Button.vue";

type ModalType = "edit" | "add";

export default defineComponent({
  name: "Modal",
  components: {
    Button,
  },
  props: {
    title: String,
    editData: Object,
    modalType: String as () => ModalType,
  },
  data() {
    return {
      task: "",
      dueDate: "",
    };
  },
  emits: ["addTask", "closeModal"],
  methods: {
    close() {
      this.$emit("closeModal");
      setTimeout(() => {
        this.resetInput();
      }, 300);
    },
    async handleSubmit() {
      const data = {
        text: this.task,
        day: this.dueDate,
      };
      this.close();
      this.$emit("addTask", data);
      setTimeout(() => {
        this.resetInput();
      }, 300);
    },
    resetInput() {
      this.task = "";
      this.dueDate = "";
    },
  },
});
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2em;
}

.modal {
  display: flex;
  flex-direction: column;
  padding: 2em;
  width: 35em;
  background: #ffffff;
  overflow-x: auto;
  text-align: center;
  border-radius: 10px;
  z-index: 10;
  user-select: none;
}

.input {
  font-family: inherit;
  font-weight: 500;
  padding: 0.6rem 4rem 0.6rem 0.8rem;
  width: 100%;
  border: 0;
  border-radius: 10px;
  outline: none;
  box-shadow: 0 0 0 3px rgba(253, 77, 77, 0.8);
}
</style>
