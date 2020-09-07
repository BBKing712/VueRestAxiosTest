<template>
  <div id="app">
    <h1>Todos</h1>
    <input type="text" v-model="todoName" @keyup.enter="addTodo">
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        {{todo.name}}<button @click="DeleteTodoById(todo.id)">Löschen</button>
        <label> </label>
        <input type="checkbox" :id="todo.id" v-model="todo.isComplete">
        <label>Erledigt</label>
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
    },
    DeleteTodoByIdOld(id)
    {
      var url = "http://localhost:57230/api/TodoItems/" +id;
      var config 
      var response = axios.delete(url, config);
      var deleted = response.data;
      var idx = -1
    if (typeof deleted !== 'undefined')
    {
       idx = this.todos.indexOf(deleted);
      if(idx != -1)
      {
        this.todos.splice(idx, 1);
      }
   }
    },
    //Arrows
      DeleteTodoByIdOld2(id)
      {
        var url = "http://localhost:57230/api/TodoItems/" +id;
        var config
        axios.delete(url, config)
  .then((response) => {
      var deleted = response.data;
      var idx = -1
    if (typeof deleted !== 'undefined')
    {
       idx = this.todos.indexOf(deleted);
      if(idx != -1)
      {
        this.todos.splice(idx, 1);
      }
  }
  })
  .catch(function (error) {
    console.log(error)
  })
      },
      //bind(this)
      DeleteTodoById(id)
      {
         var url = "http://localhost:57230/api/TodoItems/" +id;
        var config
        axios.delete(url, config)
  .then(function (response) {
     var deleted = response.data;
      var idx = -1
    if (typeof deleted !== 'undefined')
    {
      var deletedId = deleted.id;
      var item = this.todos.find(item => item.id === deletedId);
       idx = this.todos.indexOf(item);
      if(idx != -1)
      {
        this.todos.splice(idx, 1);
      }
  }
 }.bind(this))
  .catch(function (error) {
    console.log(error)
  })
       
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
