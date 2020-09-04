<template>
  <div id="app">
    <h1>Todos</h1>
    <input type="text" v-model="todoName" @keyup.enter="addTodo">
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        {{todo.name}}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

// const baseUrl = "http://localhost:57230/api/TodoItems";

export default {
  name: 'app',
  data() {
    return {
      todos: [],
      todoName: ''
    }
  },
  async created() {
    try {
      const res = await axios.get("http://localhost:57230/api/TodoItems")

      this.todos = res.data;
    } catch(e) {
      // console.error(e)
    }
  },
  methods: {
    async addTodo() {
      const res = await axios.post("http://localhost:57230/api/TodoItems", { name: this.todoName })

      this.todos = [...this.todos, res.data]

      this.todoName = ''
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  margin: 0px;
  padding: 0px;
}
li {
  list-style: none;
}
</style>
