<template>
  <main>
    <VTitle>Vue Todo List</VTitle>

    <VTodoAdd @add="addTodo" />

    <ul class="todo-list">
      <VTodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :text="todo.text"
        :checked="todo.checked"
        @check="toggleCheckStatus(index)"
        @remove="removeTodo(index)"
      />
      <li v-if="todos.length === 0">Il n'y a rien à faire !</li>
    </ul>
  </main>
</template>

<script>
import VTitle from './components/VTitle'
import VTodoAdd from './components/VTodoAdd'
import VTodoItem from './components/VTodoItem'

import { onMounted, ref, watch } from 'vue'

export default {
  name: 'App',
  components: {
    VTodoItem,
    VTodoAdd,
    VTitle
  },

  setup () {
    const todos = ref([
      { text: 'Faire les courses', checked: false }
    ])

    const addTodo = (text) => {
      const newTodo = { text, checked: false }
      todos.value = [...todos.value, newTodo]
    }

    const removeTodo = (index) => {
      const todoTemp = [...todos.value]
      todoTemp.splice(index, 1)

      todos.value = todoTemp
    }

    const toggleCheckStatus = (index) => {
      const todoTemp = [...todos.value]
      todoTemp[index].checked = !todoTemp[index].checked

      todos.value = todoTemp
    }

    // Le watch exécute la fonction à
    // chaque fois que todos est modifié
    watch(todos, (val) => {
      // On converti les todos en string
      const stringifiedTodos = JSON.stringify(val)
      // On les enregistre dans le localStorage
      window.localStorage.setItem('todos', stringifiedTodos)
    })

    // onMounted lance la fonction dès que le
    // composant est chargé
    onMounted(() => {
      // On va chercher les todos dans le localStorage
      const todosFromStorage = window.localStorage.getItem('todos')

      // On vérifie que celles-ci existent
      if (todosFromStorage) {
        // Si oui, on les réasignes aux todos de l'app
        todos.value = JSON.parse(todosFromStorage)
      }
    })

    return {
      todos,
      addTodo,
      removeTodo,
      toggleCheckStatus
    }
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 500px;
  margin: 0 auto;
}

.todo-list {
  background: #ccc;
  border-radius: 5px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  list-style: none;
}
</style>
