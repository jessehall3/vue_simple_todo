<template>
	<div>
		<BaseInputText
			v-model="newTodoText"
			placeholder="New todo"
			@keydown.enter="addTodo"
		/>
		<hr>
		<div class="">
			Mark All As Done<input type="checkbox" v-model="isAllDone" @click="toggleAllDone">
		</div>
		<div class="">
			<button type="button" name="button">Delete All</button>
		</div>
		<hr>
		<ol v-if="todos.length">
			<TodoListItem
				v-for="todo in todos"
				:key="todo.id"
				:todo="todo"
				@remove="removeTodo"
			/>
		</ol>
		<p v-else>
			Nothing left in the list. Add a new todo in the input above.
		</p>
	</div>
</template>

<script>
import BaseInputText from './BaseInputText.vue'
import TodoListItem from './TodoListItem.vue'

let nextTodoId = 1

export default {
	components: {
		BaseInputText, TodoListItem
	},
  data () {
    return {
			newTodoText: '',
			isAllDone: false,
      todos: [
				{
					id: nextTodoId++,
					text: 'Buy tomotatoes.',
					isDone: false
				},
				{
					id: nextTodoId++,
					text: 'Smell the roses.',
					isDone: false
				},
			]
    }
  },
	methods: {
		addTodo () {
			const trimmedText = this.newTodoText.trim()
			if (trimmedText) {
				this.todos.push({
					id: nextTodoId++,
					text: trimmedText
				})
				this.newTodoText = ''
			}
		},
		removeTodo (idToRemove) {
			this.todos = this.todos.filter(todo => {
				return todo.id !== idToRemove
			})
		},
		toggleAllDone () {
			const self = this
			self.todos = self.todos.map(todo => {
				todo.isDone = !self.isAllDone
				return todo
			})
		}
	}
}
</script>
