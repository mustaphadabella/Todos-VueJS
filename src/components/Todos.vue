<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une Tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li class="todo"  v-for="todo in filteredTodos" :key="filteredTodos.indexOf(todo)" :class="{completed: todo.completed, editing: todo === editing}" :data-r="filteredTodos.indexOf(todo)">
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEditing" v-focus="todo === editing" @blur="doneEditing" @keyup.esc="cancelEdit">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="haveItems">
      <span class="todo-count">
        <strong>{{remaining}}</strong> tâches a faire.
      </span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter ==='all'}" @click.prevent = "filter = 'all'">Toutes</a></li>
        <li><a href="#" :class="{selected: filter ==='todo'}" @click.prevent = "filter = 'todo'">A Faire</a></li>
        <li><a href="#" :class="{selected: filter ==='done'}" @click.prevent = "filter = 'done'">Faites</a></li>
      </ul>
      <button class="clear-completed" v-show="showClear" @click.prevent="clearSelected">Supprimer Selection</button>
    </footer>
  </section>
</template>
<script>
import Vue from 'vue'

export default {
  data () {
    return {
      todos: [],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    addTodo () {
      var value = this.newTodo && this.newTodo.trim()
      if (!value) {
        return
      }
      this.todos.unshift({name: this.newTodo, complted: false})
      this.newTodo = ''
    },
    deleteTodo (item) {
      this.todos = this.todos.filter(i => i !== item)
    },
    clearSelected () {
      this.todos = this.todos.filter(i => !i.completed)
    },
    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    doneEditing () {
      this.editing = null
    },
    cancelEdit (todo) {
      this.editing.name = this.oldTodo
      this.doneEditing()
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        if (value === true) {
          this.todos.forEach((item) => {
            item.completed = true
          })
        } else {
          this.todos.forEach((item) => {
            item.completed = false
          })
        }
      }
    },
    showClear () {
      return this.todos.filter(item => item.completed).length !== 0
    },
    haveItems () {
      return Array.isArray(this.todos) && this.todos.length !== 0
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(() => {
          el.focus()
        })
      }
    }
  }
}
</script>

<style src="./todos.css"></style>
