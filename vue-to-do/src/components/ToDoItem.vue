<template>
  <div>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title class="py-1">
          <h2>
            {{ todoItem.title }}
          </h2>
        </v-list-item-title>
        <v-list-item-subtitle class="mt-1" style="font-size: 16px">
          {{ todoItem.description }}
        </v-list-item-subtitle>

        <p style="font-size: 14px">
          {{ todoItem.date }}
        </p>
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
        v-if="todoItem.isCompleted"
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
  props: ["todoItem"],
  methods: {
    setCompleted(item) {
      this.$props.todoItem.isCompleted = true;
      console.log("setCompleted", this.$props.todoItem.isCompleted);
      appAxios.put(`/ToDoList/${item.id}`, item);
    },

    setDelete(item) {
      this.$props.todoItem.isCompleted = false;
      console.log("setDelete", this.$props.todoItem.isCompleted);
      appAxios.put(`/ToDoList/${item.id}`, item);
    },
    deleteItem(item) {
      console.log("yikes", item);
      this.$emit("deleteItem", item);
    },
  },
};
</script>
