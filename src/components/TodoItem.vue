<template>
  <p :class="['todo-item', todoProps.complete ? 'is-completed' : '']">
    <input
      type="checkbox"
      :checked="todoProps.complete"
      @change="changeComplete"
    />
    {{ todoProps.title }}
    <button class="del-btn" @click="deleteItem">Delete</button>
  </p>
</template>

<script>
export default {
  name: 'TodoItem',
  props: ['todoProps'],
  setup(props, context) {
    const changeComplete = () => {
      context.emit('item-completed', props.todoProps.id)
    }
    const deleteItem = () => {
      context.emit('item-delete', props.todoProps.id)
    }
    return { changeComplete, deleteItem }
  }
}
</script>

<style>
.del-btn {
  background: red;
  color: white;
  border: none;
  cursor: pointer;
  float: right;
}
.todo-item {
  background: #f4f4f4;
  padding: 10px;
  margin: 0;
  border-bottom: 1px #ccc dotted;
}
.is-completed {
  text-decoration: line-through;
}
</style>
