<template>
	<section id="app" class="todoapp">
		<header class="header">
			<h1>todos</h1>
			<todo-input
				class="new-todo" autofocus autocomplete="off"
				placeholder="What needs to be done?"
				@add-todo="addTodo">
			</todo-input>
		</header>
		<todo-list
			:todos="todos"
			:filtered-todos="filteredTodos"
			@remove-todo="removeTodo"
			@done="done">
		</todo-list>
		<todo-controller
			:todos="todos"
			:remaining="remaining">
		</todo-controller>
	</section>
</template>

<script>
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';
import TodoController from './components/TodoController.vue';

const STORAGE_KEY = 'todos-vuejs-2.6';
const todoStorage = {
	fetch() {
		const todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
		todos.forEach(function(todo, index) {
			todo.id = index;
		});
		todoStorage.uid = todos.length;
		return todos;
	},
	save(todos) {
		localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
	}
};
export default {
	name: 'app',
	components: {
		TodoInput,
		TodoList,
		TodoController
	},
	data() {
		return {
			todos: todoStorage.fetch()
		}
	},
	computed: {
		filteredTodos() {
			return this.todos;
		},
		remaining() {
			const todos = this.getActive(this.todos);
			return todos.length;
		}
	},
	watch: {
		todos: {
			handler(todos) {
				todoStorage.save(todos);
			},
			deep: true
		}
	},
	methods: {
		addTodo(todoTitle) {
			const newTodo = todoTitle && todoTitle.trim();
			if (!newTodo) {
				return;
			}
			this.todos.push({
				id: todoStorage.uid++,
				title: newTodo,
				completed: false
			});
		},
		removeTodo(todo) {
			this.todos = this.todos.filter((item) => item !== todo);
		},
		done(todo, completed) {
			todo.completed = completed;
		},
		getActive(todos) {
			return todos.filter((todo) =>
				!todo.completed
			);
		}
	}
}
</script>

<style>
@import url("https://unpkg.com/todomvc-app-css@2.2.0/index.css");
</style>
