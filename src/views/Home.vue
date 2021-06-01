<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import AddTodo from "../components/AddTodo";
import Todos from "../components/Todos";
import axios from "axios";

export default {
  name: "Home",
  components: {
    AddTodo,
    Todos,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    async deleteTodo(id) {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`);
        this.todos = this.todos.filter((todo) => todo.id !== id);
      }
      catch (error) {
        console.log("Delete todo error ::", error);
      }
    },
    async addTodo(newTodo) {
      try {
        const response = await axios.post("https://jsonplaceholder.typicode.com/todos", { ...newTodo });
        this.todos = [...this.todos, response.data]
      }
      catch (error) {
        console.log("Add todo error ::", error);
      }
    }
  },
  async created() {
    try {
      const response = await axios.get("https://jsonplaceholder.typicode.com/todos?_limit=5");

      this.todos = response.data;
    }
    catch (error) {
      console.log(error);
    }
  }
};
</script>

<style>
    * {
      box-sizing: border-box;
      padding: 0;
      margin: 0;
    }

    body {
      font-family: Arial, Helvetica, sans-serif;
      line-height: 1.4;
    }

    .btn {
      display: inline-block;
      border: none;
      background: #555;
      color: #fff;
      padding: 7px 20px;
      cursor: pointer;
    }

    .btn:hover {
      background: #666;
    }
</style>
