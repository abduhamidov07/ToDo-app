<template>
  <section class="container todoApp">
    <h1>Todo App</h1>
    <main class="addTodo">
      <div class="formGroup">
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="todoTitle" />
      </div>
      <div class="formGroup">
        <label for="content">Content:</label>
        <textarea id="content" v-model="newTodo"></textarea>
      </div>
      <button class="addBtn" @click="addTodo">Add</button>
    </main>
    <div class="todoList">
      <h2>Todo List</h2>
      <div class="todos">
        <div v-for="(todo, index) in todos" :key="index" class="todo">
          <div class="todoContent">
            <h3>{{ todo.title }}</h3>
            <hr />
            <div class="todoInfo">
              <p>{{ todo.content }}</p>
              <small>{{ todo.date }}</small>
              <button class="btn" @click="removeTodo(index)">Delete</button>
            </div>
          </div>
        </div>
      </div>
      <div v-if="todos.length > 0">
        <button class="removeAllBtn" @click="removeAll()">Clear all</button>
      </div>
    </div>
    <div class="theme">
      <input
        type="radio"
        id="light"
        value="light"
        v-model="selectedStyle"
        @change="changeStyle"
        @click="switchv"
      />
      <input
        type="radio"
        id="dark"
        value="dark"
        v-model="selectedStyle"
        @change="changeStyle"
        @click="switchv"
      />
      <label for="light" v-if="showElem"><i class="fa-solid fa-sun"></i></label>
      <label for="dark" v-else><i class="fa-solid fa-moon"></i></label>
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
      showElem: true,
    };
  },
  methods: {
    changeStyle() {
      const body = document.body;
      switch (this.selectedStyle) {
        case "light":
          body.classList.add("light");
          break;
        case "dark":
          body.classList.remove("light");
          break;
      }
    },
    switchv() {
      this.showElem = !this.showElem;
    },
    addTodo() {
      if (this.newTodo && this.todoTitle) {
        this.todos.push({
          done: false,
          title: this.todoTitle,
          content: this.newTodo,
          date: new Date().toLocaleDateString(),
        });
        this.newTodo = "";
        this.todoTitle = "";
        this.saveData();
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
