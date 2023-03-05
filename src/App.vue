<template>
  <main class="container">
    <div class="position-absolute end-0 mt-2 me-2 theme">
      <input
        type="radio"
        id="light"
        value="light"
        v-model="selectedStyle"
        class="d-none"
        @change="changeStyle"
        @click="switchV"
      />

      <input
        type="radio"
        id="dark"
        value="dark"
        v-model="selectedStyle"
        class="d-none ms-1"
        @change="changeStyle"
        @click="switchV"
      />
      <label for="light" v-if="showElem"
        ><i class="fa-solid fa-sun text-white fs-3"></i
      ></label>
      <label for="dark" v-else
        ><i class="fa-solid fa-moon text-dark fs-3"></i
      ></label>
    </div>
    <div class="row row-cols-md-2 d-flex justify-content-center">
      <div class="card border-0">
        <h1 class="text-light text-center">ToDo App</h1>
        <form class="createToDo d-flex flex-column" @submit.prevent="addTodo()">
          <label class="text-light mb-1">ToDo Title</label>
          <input v-model="todoTitle" name="todoTitle" autocomplete="off" />
          <label class="text-light mb-1">New ToDo </label>
          <input v-model="newTodo" name="newTodo" autocomplete="off" />
          <button class="fs-5 addBtn btn mt-3">Add ToDo</button>
        </form>
        <hr />
        <h2 class="text-light my-2 text-center">ToDo List</h2>
        <ul>
          <li v-for="(todo, index) in todos" :key="index">
            <p class="text-light fw-normal fs-5">Title: {{ todo.title }}</p>
            <hr class="my-2" />
            <div
              class="itemInfo d-flex justify-content-between align-items-center"
            >
              <p class="todoContent">
                {{ todo.content }}
              </p>
              <span class="d-flex align-items-center">
                <p class="text-secondary">{{ todo.date }}</p>
                <button
                  @click="removeTodo(index)"
                  class="btn trashBtn ms-2 text-secondary"
                >
                  <i class="fa-solid fa-trash-can"></i>
                </button>
              </span>
            </div>
          </li>
        </ul>
        <h4 v-if="todos.length == 0" class="text-secondary">Empty list.</h4>
        <button
          v-if="todos.length > 0"
          @click="removeAll(index)"
          class="removeAllBtn btn fs-5">
          Clear all
        </button>
      </div>
    </div>
  </main>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  data() {
    return {
      selectedStyle: "dark",
      showElem: true,
      // changeStyle:,
    };
  },
  methods: {
    switchV() {
      this.showElem = !this.showElem;
    },
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
  },
  setup() {
    const newTodo = ref("");
    const todoTitle = ref("");
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value && todoTitle.value) {
        todos.value.push({
          done: false,
          title: todoTitle.value,
          content: newTodo.value,
          date: new Date().toLocaleDateString(),
        });
        newTodo.value = "";
        todoTitle.value = "";
      }
      saveData();
    }
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }
    function removeAll(index) {
      todos.value.splice(0);
      saveData();
    }
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }
    return {
      todos,
      todoTitle,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      removeAll,
      saveData,
    };
  },
};
</script>
