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
      const response = await axios.get("http://localhost:57230/api/TodoItems")

      this.todos = response.data;
    } catch(e) {
      // console.error(e)
    }
  },
  methods: {
    async addTodo() {

      var todo = [
      {
        name: this.todoName,
        isComplete: false
      }
      ]
      const response = await axios.post("http://localhost:57230/api/TodoItems", todo)
      // this.todos.push(response.data);
      this.todos =  this.todos.concat(response.data);

      // this.todos = [...this.todos, response.data]

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
