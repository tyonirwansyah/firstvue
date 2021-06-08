<template>
  <div>
    <Wrapper padding="3em">
      <Header text="Welcome to VueJS + Typescript" />
      <Button
        text="Start Learning"
        target="_blank"
        href="https://v3.vuejs.org/guide/introduction.html"
      />
    </Wrapper>
    <Wrapper
      padding="3em"
      style="display: flex; align-items: center; justify-content: space-between ;margin-top: -45px"
    >
      <h3>Start by creating a to-do list</h3>
      <Button text="Add Task" size="sm" color="#095256" @click="showModal" />
    </Wrapper>
    <Wrapper padding="0 3em" style="margin-top: -25px">
      <Lists
        :lists="tasks"
        @deleteTask="deleteTask"
        @editTask="showEditModal"
      />
    </Wrapper>

    <transition name="modal-fade">
      <Modal
        title="Create a Task"
        v-show="isModalVisible"
        modalType="add"
        @closeModal="closeModal"
        @addTask="addTask"
      />
    </transition>

    <transition name="modal-fade">
      <Modal
        title="Edit Task"
        modalType="edit"
        v-show="isEditModalVisible"
        @closeModal="closeEditModal"
        @addTask="editTask"
      />
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "./components/Header.vue";
import Button from "./components/Button.vue";
import Wrapper from "./components/Wrapper.vue";
import Modal from "./components/Modal.vue";
import Lists from "./components/Lists.vue";

export default defineComponent({
  name: "App",
  emits: ["closeModal", "addTask", "deleteTask"],
  components: {
    Header,
    Button,
    Wrapper,
    Modal,
    Lists,
  },
  data() {
    return {
      isModalVisible: false,
      isEditModalVisible: false,
      tasks: [] as any,
      id: "",
      editData: {},
    };
  },
  methods: {
    async showEditModal(id: any) {
      await this.fetchTasksById();
      this.isEditModalVisible = true;
      this.id = id;
    },
    closeEditModal() {
      this.isEditModalVisible = false;
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    async fetchTasksById() {
      const res = await fetch(`http://localhost:5000/tasks/${this.id}`);
      const data = await res.json();
      return (this.editData = {
        text: data.text,
        day: data.day,
      });
    },
    async fetchTasks() {
      const res = await fetch("http://localhost:5000/tasks");
      this.tasks = await res.json();
      return this.tasks;
    },
    async deleteTask(id: any) {
      await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json",
        },
      });
      this.tasks = this.tasks.filter((task: any) => task.id !== id);
    },
    async addTask(task: object) {
      const res = await fetch(`http://localhost:5000/tasks`, {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async editTask(task: object) {
      const res = await fetch(`http://localhost:5000/tasks/${this.id}`, {
        method: "PATCH",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      for (let i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].id === this.id) {
          this.tasks[i] = data;
        }
      }
      this.id = "";
    },
  },
  created() {
    this.fetchTasks();
  },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  font-family: "Poppins", sans-serif;
}

.modal-fade-enter,
.modal-fade-leave-to {
  opacity: 0;
}
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.2s ease;
}
</style>
