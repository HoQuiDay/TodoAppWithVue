<template>
  <form @submit="addTodo">
    <input type="text" placeholder="Add new job" v-model="title" />
    <input type="submit" value="Add Todo" />
  </form>
</template>

<script>
import { ref } from 'vue'
import { v4 as uuidv4 } from 'uuid'
export default {
  name: 'AddTodo',
  setup(props, context) {
    const title = ref('')

    const addTodo = event => {
      event.preventDefault()
      const newTodo = {
        title: title.value,
        completed: false
      }

      context.emit('add-todo', newTodo)
      title.value = ''
    }
    return { title, addTodo }
  }
}
</script>

<style>
form {
  display: flex;
}
input[type='text'] {
  flex: 9;
  padding: 5px;
}
input[type='submit'] {
  flex: 3;
  border: none;
  cursor: pointer;
  background: green;
  color: aliceblue;
  font-weight: 600;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande',
    'Lucida Sans', Arial, sans-serif;
}
</style>
