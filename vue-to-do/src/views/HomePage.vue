<template>
  <div>
    <v-card class="mx-auto mt-9" outlined rounded light style="width: 720px">
      <v-toolbar flat color="green accent-2">
        <v-toolbar-title class="headline outlined">To Do List</v-toolbar-title>
        <v-spacer></v-spacer>

        <v-btn large @click="print" icon class="mr-1">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-toolbar>

      <v-list>
        <v-list-item-group v-for="todo in ToDoItemList" :key="todo">
          <ToDoItem :todoItem="todo" />
        </v-list-item-group>
      </v-list>
      <v-card-action>
        <div style="display: flex; flex-direction: row-reverse" class="ma-2">
          <v-btn text outlined class="green accent-2"> ADD NEW </v-btn>
        </div>
      </v-card-action>
    </v-card>

    <v-spacer> </v-spacer>

    <v-dialog
      @click="openAddNew"
      style="width: 720px"
      persistent
      max-width="720"
    >
      <v-row justify="center">
        <v-card>
          <h2>Hello</h2>
          <h2>Hello</h2>
          <h2>Hello</h2>
          <h2>Hello</h2>
        </v-card>
      </v-row>
    </v-dialog>
  </div>
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
      isOpenAddNew: false,
    };
  },
  methods: {
    openAddNew() {
      this.isOpenAddNew = true;
    },
  },
  components: { ToDoItem },
};
</script>
