<template>
  <AddTodo @add-todo="addTodo" />
  <TodoItem
    v-for="todo in todos"
    :key="todo.id"
    :todoProps="todo"
    @item-completed="markComplete"
    @item-delete="deleteItem"
  ></TodoItem>
</template>

<script>
import { ref } from 'vue'
import TodoItem from './TodoItem.vue'
import AddTodo from './AddTodo.vue'
import { v4 as uuidv4 } from 'uuid'
import axios from 'axios'
export default {
  name: 'Todos',
  components: { TodoItem, AddTodo },
  setup() {
    const todos = ref([])
    const getAllTodos = async () => {
      try {
        const res = await axios.get(
          'https://jsonplaceholder.typicode.com/todos?_limit=10'
        )
        if (res && res.data) todos.value = res.data
      } catch (error) {
        console.log('🚀 >>>>> getAllTodos >>>>> error:', error)
      }
    }
    getAllTodos()
    const markComplete = id => {
      todos.value = todos.value.map(todo => {
        if (todo.id === id) todo.complete = !todo.complete
        return todo
      })
    }
    const deleteItem = async id => {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        todos.value = todos.value.filter(todo => {
          return todo.id !== id
        })
      } catch (error) {
        console.log('🚀 >>>>> deleteItem >>>>> error:', error)
      }
    }
    const addTodo = async newTodo => {
      try {
        const res = await axios.post(
          `https://jsonplaceholder.typicode.com/todos`,
          newTodo
        )
        todos.value.push(res.data)
      } catch (error) {
        console.log('🚀 >>>>> addTodo >>>>> error:', error)
      }
    }

    return { todos, markComplete, deleteItem, addTodo }
  }
}
</script>

<style></style>
