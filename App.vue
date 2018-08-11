<template>
  <div class="container" id="app">
    <div class="form-inline">
      <input class="form-control" v-model="input"></input>
      <a class="btn btn-primary" v-on:click="add">add</a>
      <!-- <button @click="add()">add</button> -->
    </div>
    <table class="table">
    <thead>
    <tr> 
      <th>todo</th>
      <th>action</th>
    </tr>
    </thead>
    <tbody>
      <tr v-for="(todo, id) in todos" :class="todo.completed ? 'success' : ''">
        <td>{{ todo.content }}</td>
        <td>
         <a class="btn btn-success btn-xs" v-if="!todo.completed" v-on:click="complete(id)">✔</a>
         <a class="btn btn-danger btn-xs" v-if="todo.completed" v-on:click="remove(id)">✘</a>
        </td>
      </tr>
    </tbody>
    </table>
      
  </div>
</template>

<script>
export default {
  // name: 'App'
  data: function(){
    return{
      input: '',
      todos: [],
    }
  },
  created() {
    this.getData()
  },
  methods: {
    getData() {
      fetch('http://127.0.0.1:9000/api/list')
        .then(data => data.json())
        .then(data => this.todos = data)
    },
    add() {
      fetch(`http://127.0.0.1:9000/api/add?content=${this.input}`, {method: 'POST'})
        .then(() => this.getData()),
      this.input=''
    },
    remove(id) {
      fetch(`http://127.0.0.1:9000/api/item/${id}`, {method: 'DELETE'})
        .then(() => this.getData())
    },
    complete(id) {
      fetch(`http://127.0.0.1:9000/api/item/${id}/marktocomplete`, {method: 'PUT'})
        .then(() => this.getData())
    }
  }
}
</script>

<style>
/* .finished{
  text-decoration: underline
} */
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
