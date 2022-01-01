<template>
  <div>
    <v-card class="mx-auto mt-9" outlined rounded light style="width: 720px">
      <v-toolbar flat color="green accent-2">
        <v-toolbar-title class="headline outlined text-h4 font-weight-bold mt-1"
          >To Do List
        </v-toolbar-title>
        <v-spacer></v-spacer>

        <v-btn large @click="print" icon class="mr-1">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-toolbar>

      <v-list>
        <v-list-item-group v-for="todo in ToDoItemList" :key="todo">
          <ToDoItem :todoItem="todo" @deleteItem="deleteItem($event)" />
        </v-list-item-group>
      </v-list>
      <v-card-action>
        <div style="display: flex; flex-direction: row-reverse" class="ma-2">
          <v-btn
            text
            outlined
            class="green accent-2"
            @click="isOpenAddNew = true"
          >
            ADD NEW
          </v-btn>
        </div>
      </v-card-action>
    </v-card>

    <v-spacer> </v-spacer>
    <!-- Start New To Do -->
    <v-dialog
      v-model="isOpenAddNew"
      persistent
      width="540px"
      hide-overlay
      ref="openAddNewModal"
      class="mx-auto"
      style="overflow: hidden"
      scrollable
    >
      <v-card height="620px" rounded>
        <v-system-bar height="62px" width="540px" color="green accent-2">
          <p class="text-h4 font-weight-bold mt-4">New To Do</p>
        </v-system-bar>
        <v-card-actions>
          <v-col>
            <v-form>
              <v-row>
                <v-col>
                  <v-text-field
                    id="title"
                    clearable
                    full-width
                    label="Title"
                    class="mt-2"
                    v-model="newItem.title"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <v-text-field
                clearable
                full-width
                id="date"
                v-model="newItem.date"
                label="Deadline"
                class="mt-2"
              >
              </v-text-field>
              <v-spacer></v-spacer>
              <v-row class="mt-2">
                <v-col>
                  <v-textarea
                    full-width
                    dense
                    label="Describe your new task"
                    counter
                    clearable
                    auto-grow
                    outlined
                    height="180px"
                    v-model="newItem.description"
                    id="description"
                  >
                  </v-textarea>
                </v-col>
              </v-row>
            </v-form>
          </v-col>
        </v-card-actions>

        <v-spacer></v-spacer>
        <v-card-actions>
          <v-row style="display: flex; flex-direction: row-reverse">
            <div class="ma-2">
              <v-btn text outlined class="white" @click="isOpenAddNew = false">
                CLOSE
              </v-btn>
            </div>
            <div class="ma-2">
              <v-btn
                text
                outlined
                class="green accent-2"
                @click="(isOpenAddNew = false), onSubmit()"
                type="submit"
              >
                SAVE
              </v-btn>
            </div>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- End New To Do Dialog -->
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
      newItem: {
        title: null,
        description: null,
        date: null,
        isCompleted: null,
        createdAt: null,
        id: null,
      },
    };
  },
  methods: {
    openAddNew() {
      this.isOpenAddNew = true;
    },
    onSubmit() {
      const saveItem = { ...this.newItem };
      saveItem.isCompleted = false;
      saveItem.createdAt = new Date().toDateString();
      saveItem.id = this.ToDoItemList.length + 1;
      console.log(saveItem);
      console.log("mert");
      appAxios.post("/ToDoList", saveItem).then((res) => {
        console.log(res.data);
        this.clearModal(), this.ToDoItemList.push(saveItem);
      });
    },
    clearModal() {
      const clearModal = {
        title: null,
        description: null,
        date: null,
        isCompleted: null,
        createdAt: null,
      };
      this.newItem = clearModal;
    },
    deleteItem(item) {
      console.log("mert", item);
      appAxios
        .delete(`/ToDoList/${item.id}`)
        .then(
          (this.ToDoItemList = this.ToDoItemList.filter((i) => i.id != item.id))
        );
    },
  },
  components: { ToDoItem },
};
</script>
