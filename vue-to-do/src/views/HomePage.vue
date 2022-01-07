<template>
  <div>
    <v-card class="mx-auto mt-9" outlined rounded light style="width: 720px">
      <v-toolbar flat color="green accent-2">
        <v-toolbar-title class="headline outlined text-h4 font-weight-bold mt-1"
          >To Do List
        </v-toolbar-title>
        <v-spacer></v-spacer>

        <v-btn
          large
          @click="(isSearchOpen = true), (searchList = ToDoItemList)"
          icon
          class="mr-1"
        >
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-toolbar>
      <v-text-field
        class="mx-4 mb-4"
        clearable
        v-if="isSearchOpen"
        hide-details
        single-line
        append-outer-icon="mdi-close-box-outline"
        label="Search"
        color="black"
        @click:append-outer="
          (isSearchOpen = false), (searchString = null), (searchList = null)
        "
        v-model="searchString"
        @keypress="searchToDo"
      ></v-text-field>
      <v-list v-if="!isSearchOpen">
        <transition-group
          class="fade"
          enter-active-class="animate__animated animate__flipInX"
          leave-active-class="animate_animated animate__flipOutX"
        >
          <v-list-item-group v-for="todo in ToDoItemList" :key="todo">
            <ToDoItem
              :todoItem="todo"
              @deleteItem="deleteItem($event)"
              @add-update-completed="addCompleted($event)"
            />
          </v-list-item-group>
        </transition-group>
      </v-list>
      <v-list v-if="isSearchOpen">
        <v-list-item-group v-for="todo in searchList" :key="todo.id">
          <ToDoItem
            :todoItem="todo"
            @deleteItem="deleteItem($event)"
            @add-update-completed="addCompleted($event)"
          />
        </v-list-item-group>
      </v-list>
      <v-toolbar flat color="green accent-2">
        <v-toolbar-title
          class="headline outlined text-h4 font-weight-bold mt-1"
        >
          Completed
        </v-toolbar-title>
      </v-toolbar>

      <v-list>
        <transition-group
          class="fade"
          enter-active-class="animate__animated animate__flipInX"
          leave-active-class="animate_animated animate__flipOutX"
        >
          <v-list-item-group v-for="ctodo in completedTodoList" :key="ctodo.id">
            <ToDoItem
              :todoItem="ctodo"
              @deleteItem="deleteItem($event)"
              @delete-update-completed="deleteCompleted($event)"
            />
          </v-list-item-group>
        </transition-group>
      </v-list>

      <v-card-actions>
        <v-spacer></v-spacer>
        <div class="ma-2">
          <v-btn
            text
            outlined
            class="green accent-2"
            @click="isOpenAddNew = true"
          >
            ADD NEW
          </v-btn>
        </div>
      </v-card-actions>
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
          <v-row style="display: flex; flex-direction: row-reverse;">
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
            <div class="ma-2">
              <v-btn text outlined class="white" @click="isOpenAddNew = false">
                CLOSE
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
import { nanoid } from "nanoid";
import ToDoItem from "./../components/ToDoItem.vue";


export default {
  name: "HomePage",
  created() {
    appAxios
      .get("/ToDoList")
      .then((res) => {
        this.ToDoItemList = res.data;
      })
      .then(() => {
        this.completedTodoList = this.ToDoItemList.filter(
          (i) => i.isCompleted == true
        );
        this.ToDoItemList = this.ToDoItemList.filter(
          (i) => i.isCompleted == false
        );
      });
  },
  data() {
    return {
      ToDoItemList: [],
      searchList: [],
      completedTodoList: [],
      isOpenAddNew: false,
      newItem: {
        title: null,
        description: null,
        date: null,
        isCompleted: null,
        createdAt: null,
        id: null,
      },
      isSearchOpen: false,
      searchString: null,
    };
  },
  methods: {
    openAddNew() {
      this.isOpenAddNew = true;
    },
    addCompleted(item) {
      console.log("add completed");

      this.ToDoItemList = this.ToDoItemList.filter((i) => i.id != item.id);
      this.searchList = this.searchList.filter((i)=>  i.id!=item.id);
      this.completedTodoList.push(item);
    },
    deleteCompleted(item) {
      console.log("delete completed");

      this.completedTodoList = this.completedTodoList.filter(
        (i) => i.id != item.id
      );
      this.ToDoItemList.push(item);
    },
    onSubmit() {
      const saveItem = { ...this.newItem };
      saveItem.isCompleted = false;
      saveItem.createdAt = new Date().toDateString();
      saveItem.id = nanoid();
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
    searchToDo() {
      this.searchList = this.ToDoItemList;
      this.searchList = this.ToDoItemList.filter((i) =>
        i.title.toLowerCase().includes(this.searchString.toLowerCase())
      );
    },
  },
  components: { ToDoItem },
};
</script>
<style>

.animate__animated {
  animation-duration: 0.5s;
}
</style>
