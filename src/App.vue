<template>
	<div class="app">
		<div class="app__wrapper">
			<todo-list
				:todoList="todoList"
				@update="updateTodo"
				@createTodo="createTodo"
				@removeTodo="removeTodo"
			/>
			<filter-list/>
		</div>
	</div>
</template>

<script>
import TodoList from '@/components/TodoList.vue'
import FilterList from '@/components/FilterList.vue'
import axios from 'axios'

export default {
	name: 'App',
	components: { TodoList, FilterList },
	data() {
		return {
			todoList: []
		}
	},
	mounted() {
		this.fetchTodoList()
	},
	methods: {
		async fetchTodoList() {
			try {
				const response = await axios.get('http://localhost:8000/api/todo/')
				this.todoList = response.data
			} catch (err) {
				alert(err)
			}
		},
		async updateTodo(newTodo) {
			try {
				const response = await axios.put(
					`http://localhost:8000/api/todo/${newTodo.id}/`, 
					newTodo
				)

				this.todoList.forEach((todo, idx) => {
					if (todo.id === response.data.id) {
						this.todoList[idx] = response.data
					}
				})
			} catch (err) {
				alert(err)
			}
		},
		async createTodo(newTodo) {
			try {
				const response = await axios.post(
					`http://localhost:8000/api/todo/`, 
					newTodo
				)

				this.todoList.push(response.data)
			} catch (err) {
				alert(err)
			}
		},
		async removeTodo(id) {
			await axios.delete(`http://localhost:8000/api/todo/${id}/`)
				.then(() => {
					this.todoList = this.todoList.filter(f => f.id !== id)
				})
				.catch(function (err) {
					alert(err)
				})
		}
	}
}
</script>

<style>
body {
	margin: 0;
	padding: 0;
	background-color: #181818;
}
* {
	outline: none;
	color: #eee;
	box-sizing: border-box;
	font-family: sans-serif;
	user-select: none;
}
.app__wrapper {
	display: flex;
	max-width: 1000px;
	width: 100%;
	margin: auto;
}
</style>
