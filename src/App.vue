<template>
	<div class="app">
		<div class="app__wrapper">
			<todo-search v-model="searchQuery"></todo-search>
			<filter-list/>

			<todo-list
				:todoList="sortedAndSearchedPosts"
				@update="updateTodo"
				@createTodo="createTodo"
				@removeTodo="removeTodo"
				@inputSearch="inputSearch"
			/>
		</div>
	</div>
</template>

<script>
import TodoSearch from '@/components/TodoSearch'
import TodoList from '@/components/TodoList.vue'
import FilterList from '@/components/FilterList.vue'
import axios from 'axios'

export default {
	name: 'App',
	components: { TodoSearch, TodoList, FilterList },
	data() {
		return {
			searchQuery: '',
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
	},
	computed: {
		sortedPost() {
			return this.todoList
		},
		sortedAndSearchedPosts() {
			const searchByTitle = this.sortedPost.filter(todo => todo.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
			const searchByDescription = this.sortedPost.filter(todo => todo.description.toLowerCase().includes(this.searchQuery.toLowerCase()))
		
			const result = [...searchByTitle, ...searchByDescription]
			
			const uniq = new Set(result.map(e => JSON.stringify(e)));
			const res = Array.from(uniq).map(e => JSON.parse(e));

			return res
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
	max-width: 600px;
	width: 100%;
	margin: auto;
	margin-top: 60px;
}
</style>
