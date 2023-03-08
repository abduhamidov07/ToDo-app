<template>
  <div class="container">
    <header>
      <h1>Todo App</h1>
    </header>
    <main class="add-todo">
      <h2>Add a Todo</h2>
      <div class="form-group">
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="todoTitle" />
      </div>
      <div class="form-group">
        <label for="content">Content:</label>
        <textarea id="content" v-model="newTodo"></textarea>
      </div>
      <button class="btn" @click="addTodo">Add</button>
    </main>
    <section class="todo-list">
      <h2>Todo List</h2>
      <div class="todos">
        <div v-for="(todo, index) in todos" :key="index" class="todo">
          <div class="todo-title">
            <h3>{{ todo.title }}</h3>
            <button class="btn" @click="removeTodo(index)">Delete</button>
            <button class="btn" @click="doneTodo(todo)">{{ todo.done ? 'Undone' : 'Done' }}</button>
          </div>
          <div class="todo-content">
            <p>{{ todo.content }}</p>
            <p>{{ todo.date }}</p>
          </div>
        </div>
      </div>
      <div v-if="todos.length > 0">
        <button class="btn" @click="removeAll()">Remove All</button>
      </div>
    </section>
  </div>
</template>

<script>
import { ref } from "vue";

const defaultData = [
  {
    done: false,
    title: "Example Todo",
    content: "This is an example todo.",
    date: new Date().toLocaleDateString(),
  },
];

export default {
  name: "App",
  data() {
    return {
      todoTitle: "",
      newTodo: "",
    };
  },
  methods: {
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
    doneTodo(todo) {
      todo.done = !todo.done;
      this.saveData();
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