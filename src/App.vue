<template>
	<section id="app" class="todoapp">
		<header class="header">
			<transition appear name="todo-head">
				<h1>todos</h1>
			</transition>
			<todo-input
				class="new-todo" autofocus autocomplete="off"
				placeholder="What needs to be done?"
				@add-todo="addTodo">
			</todo-input>
		</header>
		<todo-list
			:todos="todos"
			:filtered-todos="filteredTodos"
			:allDone="allDone"
			@remove-todo="removeTodo"
			@done="done"
			@allDone="onAllDone">
		</todo-list>
		<todo-controller
			:todos="todos"
			:remaining="remaining"
			:visibility="visibility"
			@removeCompleted="removeCompleted">
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
const filters = {
	all(todos) {
		return todos;
	},
	active(todos) {
		return todos.filter((todo) =>
			!todo.completed
		);
	},
	completed(todos) {
		return todos.filter((todo) =>
			 todo.completed
		);
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
			todos: todoStorage.fetch(),
			visibility: 'all'
		}
	},
	computed: {
		filteredTodos() {
			return filters[this.visibility](this.todos);
		},
		remaining() {
			const todos = filters.active(this.todos);
			return todos.length;
		},
		allDone: {
			get() {
				return this.remaining === 0;
			},
			set(value) {
				this.todos.forEach((todo) =>
					todo.completed = value
				);
			}
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
	mounted() {
		window.addEventListener('hashchange', this.onHashChange);
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
		onHashChange() {
			const visibility = window.location.hash.replace(/#\/?/, '');
			this.visibility = visibility;
		},
		onAllDone(done) {
			this.allDone = done;
		},
		removeCompleted() {
			this.todos = filters.active(this.todos);
		}
	}
}
</script>

<style>
@import url("https://unpkg.com/todomvc-app-css@2.2.0/index.css");
</style>

<style scoped>
.todo-head-enter-active {
	transition: 1s;
}
.todo-head-enter {
	opacity: 0;
	transform: translateY(-40px);
}
</style>