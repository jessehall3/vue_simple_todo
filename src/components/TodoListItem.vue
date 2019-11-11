<template>
  <div>
  <li>
    <div>
      <span v-if="this.editable">
        <BaseInputText
        v-if="this.editable"
        v-model="updatedTodoText"
        @keydown.enter="updateTodo"
        />
      </span>
      <span :class="{done: todo.isDone}" v-else>
        {{ todo.text }}
      </span>
    </div>
    <button @click="editTodo">
      {{ this.editable ? 'cancel' : 'edit'}}
    </button>
    <button @click="$emit('remove', todo.id)">
      delete
    </button>
    <input type="checkbox" v-model="todo.isDone">
  </li>
</div>
</template>

<script>
import BaseInputText from './BaseInputText.vue'

export default {
  components: {
    BaseInputText
  },
  data () {
    return {
      updatedTodoText: this.todo.text,
      editable: false,
    }
  },
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    editTodo () {
      this.editable = !this.editable
    },
    updateTodo () {
			this.todo.text = this.updatedTodoText
      this.editable = false
		},
  }
}
</script>

<style scoped>
  .done {
    color: green;
    text-decoration: line-through;
  }
</style>
