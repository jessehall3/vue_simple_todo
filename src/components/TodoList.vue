<template>
	<div>
		<BaseInputText
			v-model="newTodoText"
			placeholder="New todo"
			@keydown.enter="addTodo"
		/>
		<hr>
		<div class="">
			Mark All As Done<input type="checkbox" v-model="isAllDone">
		</div>
		<div class="">
			<button type="button" @click="deleteAllTodos">Delete All</button>
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
	computed: {
		remainingTodos: function() {
			return this.todos.filter( todo => todo.isDone === false)
		},
		isAllDone: {
			get: function() {
				return this.remainingTodos.length === 0;
			},
			set: function(value) {
				this.todos.forEach( todo => { todo.isDone = value })
			},
		}
	},
	methods: {
		addTodo () {
			const trimmedText = this.newTodoText.trim()
			if (trimmedText) {
				this.todos.push({
					id: nextTodoId++,
					text: trimmedText,
					isDone: false,
				})
				this.newTodoText = ''
			}
		},
		removeTodo (idToRemove) {
			this.todos = this.todos.filter(todo => {
				return todo.id !== idToRemove
			})
		},
		deleteAllTodos() {
			const isApproved = confirm("Are you sure you want to delete all your To-Do items?")
			if (isApproved){
				this.todos = []
			}
		}
	}
}
</script>
