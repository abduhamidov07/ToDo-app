<template>
  <main class="main">
    <div class="todoApp">
      <h1 class="siteTitle">Todo App</h1>
      <form class="addTodo" @submit.prevent="addTodo">
        <div class="formGroup">
          <label for="title">Title:</label>
          <input type="text" id="title" v-model="newTodoTitle" />
        </div>
        <div class="formGroup">
          <label for="content">Content:</label>
          <textarea id="content" v-model="newTodoText"></textarea>
        </div>
        <button class="addBtn" @click="addTodo" type="submit">Add</button>
      </form>
      <hr>
      <div class="todoList">
        <h2>Todo list</h2>
      <h3 v-if="todos.length == 0">Empty list</h3>
      <transition-group tag="ul" name="slide" class="todos">
        <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
          <div v-if="index === editingIndex" class="todoContent">
            <form @submit.prevent="saveEdit(index)">
              <input
                class="editInput"
                type="text"
                v-model="todo.title"
                required
              />
              <hr />
              <div class="todoInfo">
                <textarea class="editTextarea" v-model="todo.text" required />
                <span>
                  <button class="editBtns" type="submit">Save</button>
                  <button class="editBtns" type="button" @click="cancelEdit">
                    Cancel
                  </button>
                </span>
              </div>
            </form>
          </div>
          <div v-else class="todoContent">
            <h5>
              Title: <span>{{ todo.title }}</span>
            </h5>
            <hr />
            <div class="todoInfo">
              <p>{{ todo.text }}</p>
              <span>
                <small>{{ todo.date }}</small>
                <button class="editBtn" @click="startEdit(index)">
                  <i class="fa-solid fa-pen-to-square"></i>
                </button>
                <button class="deleteBtn" @click="removeTodo(index)">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
              </span>
            </div>
          </div>
        </li>
      </transition-group>
      <button
        v-if="todos.length > 0"
        class="deleteAllBtn"
        @click="removeTodos(index)"
      >
        Clear All
      </button>
      </div>
    </div>
    <div class="theme">
      <input
        type="radio"
        id="light"
        value="light"
        v-model="selectedStyle"
        @change="changeStyle"
        @click="switchV"
      />
      <input
        type="radio"
        id="dark"
        value="dark"
        v-model="selectedStyle"
        @change="changeStyle"
        @click="switchV"
      />
      <label for="light" v-if="showElem"
        ><i class="fa-solid fa-moon"></i
      ></label>
      <label for="dark" v-else><i class="fa-solid fa-sun"></i></label>
    </div>
  </main>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      newTodoTitle: "",
      newTodoText: "",
      editingIndex: -1,
      selectedStyle: "dark",
      showElem: true,
    };
  },
  computed: {
    todos() {
      return this.$store.state.todos;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodoText.trim() === "") {
        return;
      }
      const newTodo = {
        id: new Date().getTime(),
        title: this.newTodoTitle,
        text: this.newTodoText,
        date: new Date().toLocaleDateString(),
        done: false,
      };
      this.$store.commit("addTodo", newTodo);
      this.newTodoTitle = "";
      this.newTodoText = "";
    },
    removeTodo(index) {
      this.$store.commit("removeTodo", index);
    },
    removeTodos(index) {
      this.$store.commit("removeTodos", index);
    },
    startEdit(index) {
      this.editingIndex = index;
    },
    saveEdit(index) {
      const todo = this.todos[index];
      if (todo.text.trim() === "") {
        return;
      }
      this.$store.commit("updateTodo", { index, todo });
      this.editingIndex = -1;
    },
    cancelEdit() {
      this.editingIndex = -1;
    },
    switchV() {
      this.showElem = !this.showElem;
    },
    changeStyle() {
      const body = document.body;
      switch (this.selectedStyle) {
        case "light":
          body.classList.add("dark");
          body.classList.remove("light");
          break;
        case "dark":
          body.classList.remove("dark");
          body.classList.add("light");
          break;
      }
    },
  },
  mounted() {
    this.$store.dispatch("loadTodos");
  },
};
</script>
