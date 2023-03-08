<template>
  <section class="main">
    <div class="todoApp">
      <h1 class="siteTitle">Todo App</h1>
      <div class="addTodo">
        <div class="formGroup">
          <label for="title">Title:</label>
          <input type="text" id="title" v-model="todoTitle" />
        </div>
        <div class="formGroup">
          <label for="content">Content:</label>
          <textarea id="content" v-model="newTodo"></textarea>
        </div>
        <button class="addBtn" @click="addTodo">Add</button>
      </div>
      <hr />
      <div class="todoList">
        <h2>Todo List</h2>
        <h3 v-if="todos.length == 0">Empty list</h3>
        <div class="todos">
          <div v-for="(todo, index) in todos" :key="index" class="todo">
            <div class="todoContent">
              <h5>
                Title: <span>{{ todo.title }}</span>
              </h5>
              <hr />
              <div class="todoInfo">
                <p>{{ todo.content }}</p>
                <span>
                  <small>{{ todo.date }}</small>
                  <button class="deleteBtn" @click="removeTodo(index)">
                    <i class="fa-solid fa-trash-can"></i>
                  </button>
                </span>
              </div>
            </div>
          </div>
        </div>
        <div v-if="todos.length > 0">
          <button class="removeAllBtn" @click="removeAll()">Clear all</button>
        </div>
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
      <label for="light" v-if="showElem"><i class="fa-solid fa-moon"></i></label>
      <label for="dark" v-else><i class="fa-solid fa-sun"></i></label>
    </div>
  </section>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  data() {
    return {
      todoTitle: "",
      newTodo: "",
      selectedStyle: "dark",
      showElem: true,
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo && this.todoTitle) {
        this.todos.unshift({
          title: this.todoTitle,
          content: this.newTodo,
          date: new Date().toLocaleDateString(),
        });
        this.newTodo = "";
        this.todoTitle = "";
        this.saveData();
      }
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
    removeTodo(index) {
      this.todos.splice(index, 1);
      this.saveData();
    },
    removeAll() {
      this.todos.splice(0);
      this.saveData();
    },
    saveData() {
      const storageData = JSON.stringify(this.todos);
      localStorage.setItem("todos", storageData);
    },
  },
  setup() {
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);

    return {
      todos,
    };
  },
};
</script>
