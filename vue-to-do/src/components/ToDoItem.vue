<template>
  <div>
    <v-list-item :color="isChecked">
      <v-list-item-content>
        <v-list-item-title class="py-1">
          <h2>{{ todoItem.title }}</h2>
        </v-list-item-title>
        <v-list-item-subtitle class="mt-1" style="font-size: 16px">
          <div style="display: flex; justify-content: space-between">
            <p>
              {{ todoItem.description }}
            </p>
          </div>
        </v-list-item-subtitle>
      </v-list-item-content>

      <v-spacer></v-spacer>

      <v-btn
        large
        v-if="!todoItem.isCompleted"
        icon
        @click="setCompleted(todoItem)"
      >
        <v-icon>mdi-checkbox-marked-outline </v-icon></v-btn
      >
      <v-btn
        large
        icon
        v-else-if="todoItem.isCompleted"
        @click="setDelete(todoItem)"
      >
        <v-icon>mdi-close-box-outline </v-icon></v-btn
      >

      <v-btn large icon @click="deleteItem(todoItem)">
        <v-icon>mdi-delete </v-icon></v-btn
      >
    </v-list-item>
  </div>
</template>
<script>
import { appAxios } from "./../utils/axios";
export default {
  name: "ToDoItem",
  props: ["todoItem", "index"],
  emits: ["deleteItem", "add-update-completed", "delete-update-completed"],

  computed: {
    isChecked() {
      return this.$props.todoItem.isCompleted ? "green" : "red";
    },
  },
  methods: {
    setCompleted(item) {
      this.$props.todoItem.isCompleted = true;
      console.log("setCompleted", this.$props.todoItem.isCompleted);
      appAxios.put(`/ToDoList/${item.id}`, item).then(() => {
        this.addCompletedList(item);
      });
    },

    setDelete(item) {
      this.$props.todoItem.isCompleted = false;
      console.log("setDelete", this.$props.todoItem.isCompleted);
      appAxios.put(`/ToDoList/${item.id}`, item).then(() => {
     
        this.deleteCompleted(item);
      });
    },
    deleteItem(item) {
      this.$emit("deleteItem", item);
    },
    addCompletedList(item) {
      this.$emit("add-update-completed", item);
    },
    deleteCompleted(item) {
      this.$emit("delete-update-completed", item);
    },
  },
};
</script>
