<template>
  <div>
  <li>
    <div v-if="this.editable">
      <BaseInputText
        v-if="this.editable"
        v-model="updatedTodoText"
        @keydown.enter="updateTodo"
      />
    </div>
    <div v-else>
      {{ todo.text }}
    </div>
    <button v-on:click="editTodo">
      {{ this.editable ? 'cancel' : 'edit'}}
    </button>
    <button @click="$emit('remove', todo.id)">
      delete
    </button>
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
