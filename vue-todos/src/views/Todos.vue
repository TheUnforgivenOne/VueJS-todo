<template>
  <div>
    <router-link to="/">Home</router-link>
    <AddTodo
        @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <hr />
    <Loader v-if="loading" />
    <TodoList
        v-else-if="todos.length"
        v-bind:todos="filteredTodos"
        @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
  import AddTodo from '@/components/AddTodo'
  import TodoList from '@/components/TodoList'
  import Loader from '@/components/Loader'

  export default {
    name: 'App',
    components: {
      AddTodo,
      Loader,
      TodoList
    },
    data() {
      return {
        todos: [],
        loading: true,
        filter: 'all'
      }
    },
    mounted() {
      fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
        .then(response => response.json())
        .then(json => {
          setTimeout(() => {
            this.todos = json;
            this.loading = false;
          }, 1000)
        })
    },
    methods: {
      addTodo(todo) {
        this.todos.push(todo)
      },
      removeTodo(id) {
        this.todos = this.todos.filter(t => t.id !== id)
      }
    },
    // watch: {
    //   filter(value) {
    //     console.log(value);
    //   }
    // },
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
    }
  }
</script>
