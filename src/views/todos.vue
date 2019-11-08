
<template>
  <div>
    <h1>Todo app</h1>
    <addTodo @add-todo="addTodo"/>
    <hr>
    <select v-model="filter">
        <option value="all">all</option>
        <option value="completed">completed</option>
        <option value="not-completed">not-completed</option>
    </select>
    <loader v-if="loading"/>
    <TodoList 
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos" 
        @remove-todo="removeTodo"
    />
    <p v-else>No todo!</p>
  </div>
</template>

<script>
import TodoList from "@/components/TodoList"
import addTodo from "@/components/addTodo"
import loader from "@/components/loader"
export default {
  data() {
    return {
      todos: [
        {id: 1, title: 'Buy bread', completed: false},
        {id: 2, title: 'Buy butter', completed: false},
        {id: 3, title: 'Buy beer', completed: false},
      ],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
    .then(response => response.json())
    .then(json => {
        this.todos = json;
        this.loading = false;
    })
  },
//   watch() {
//       filter(value) {
//           console
//       }
//   }
    computed: {
        filteredTodos() {
            if (this.filter === 'all') {
                return this.todos
            }

            if (this.filter === 'completed') {
                return this.todos.filter(t => t.completed)
            }

            if (this.filter === 'not-completed') {
                return this.todos.filter(t => !t.completed)
            }
        }
    },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo)
    }
  },
  components: {
    TodoList,
    addTodo,
    loader
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
