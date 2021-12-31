<template>
  <v-card class="mx-auto mt-9" outlined rounded light style="width: 720px">
    <v-toolbar flat color="green accent-2">
      <v-toolbar-title class="headline outlined">To Do List</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-btn large @click="print" icon class="mr-1">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-toolbar>

    <v-list dense>
      <v-list-item-group v-for="todo in ToDoItemList" :key="todo">
        <ToDoItem :todoItem=todo />
      </v-list-item-group>
    </v-list>
  </v-card>
</template>
<script>
import { appAxios } from "./../utils/axios";
import ToDoItem from "./../components/ToDoItem.vue";

export default {
  mounted() {
    appAxios.get("/ToDoList").then((res) => {
      this.ToDoItemList = res.data;
    });
  },
  data() {
    return {
      ToDoItemList: [],
    };
  },
  methods: {
    print() {
      console.log("mert2", this.ToDoItemList);
    },
  },
  components: { ToDoItem },
};
</script>
