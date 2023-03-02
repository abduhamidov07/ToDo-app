<template>
  <div id="app" class="mt-4 d-flex justify-content-center">
    <div class="d-flex flex-column align-items-center">
      <h1 class="text-light">ToDo App</h1>
      <form class="createToDo d-flex flex-column" @submit.prevent="addTodo()">
        <label class="text-light mb-1">New ToDo </label>
        <input v-model="newTodo" name="newTodo" autocomplete="off" />
        <button class="fs-5 addBtn btn text-white mt-3">Add ToDo</button>
      </form>
      <hr />
      <h2 class="text-light mb-2">ToDo List</h2>
      <ul class="todoList p-0">
        <li v-for="(todo, index) in todos" :key="index">
          <span :class="{ done: todo.done }" @click="doneTodo(todo)"
            >{{ todo.content }}
          </span>
          <span class="d-flex align-items-center">
           <p class="text-secondary mb-0">{{ todo.date }}</p>
            <button @click="removeTodo(index)" class="btn trashBtn ms-2 text-secondary"><i class="fa-solid fa-trash-can"></i></button>
          </span>
        </li>
      </ul>
      <h4 v-if="todos.length == 0" class="text-secondary">Empty list.</h4>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Write a blog post",
      },
    ];
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
          date: new Date().toLocaleDateString(),
        });
        newTodo.value = "";
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
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>
