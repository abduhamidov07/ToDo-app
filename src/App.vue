<template>
  <section class="main">
    <div class="todoApp">
      <h1 class="siteTitle">Todo App</h1>
      <form class="addTodo" @submit.prevent="addTodo">
        <div class="formGroup">
          <label for="title">Title:</label>
          <input type="text" id="title" v-model="todoTitle" />
        </div>
        <div class="formGroup">
          <label for="content">Content:</label>
          <textarea id="content" v-model="newTodo"></textarea>
        </div>
        <button class="addBtn" @click="addTodo">Add</button>
      </form>

      <div class="todoList">
        <h2>Todo list</h2>
        <h3 v-if="todos.length == 0">Empty list</h3>
        <ul class="todos">
          <li v-for="todo in todos" :key="todo.id" class="todo">
            <div class="todoContent">
              <h5>
                Title: <span>{{ todo.title }}</span>
              </h5>
              <hr />
              <div class="todoInfo">
                <p>{{ todo.content }}</p>
                <span>
                  <small>{{ todo.date }}</small>
                  <button class="deleteBtn" @click="deleteTodo(todo)">
                    <i class="fa-solid fa-trash-can"></i>
                  </button>
                </span>
              </div>
            </div>
          </li>
        </ul>
        <div>
          <button
            v-if="todos.length > 0"
            class="deleteAllBtn"
            @click="deleteAll(todo)"
          >
            Clear all
          </button>
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
      <label for="light" v-if="showElem"
        ><i class="fa-solid fa-moon"></i
      ></label>
      <label for="dark" v-else><i class="fa-solid fa-sun"></i></label>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  data() {
    return {
      todos: [],
      newTodo: "",
      selectedStyle: "dark",
      showElem: true,
    };
  },
  created() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      const todos = JSON.parse(localStorage.getItem("todos")) || [];
      this.todos = todos;
    },
    saveTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    addTodo() {
      if (this.newTodo.trim()) {
        const newTodo = {
          id: Date.now(),
          title: this.todoTitle,
          content: this.newTodo,
          date: new Date().toLocaleDateString(),
        };
        this.todos.unshift(newTodo);
        this.saveTodos();
        this.todoTitle = "";
        this.newTodo = "";
      }
    },
    deleteTodo(todo) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${todo.id}`)
        .then(() => {
          this.todos.splice(this.todos.indexOf(todo), 1);
          this.saveTodos();
        });
      const Toast = Swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener("mouseenter", Swal.stopTimer);
          toast.addEventListener("mouseleave", Swal.resumeTimer);
        },
      });

      Toast.fire({
        icon: "success",
        title: "Deleted successfully",
      });
    },
    deleteAll(todo) {
      this.todos.splice(0);
      this.saveTodos();
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
};
</script>
