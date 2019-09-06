<template>
	<footer class="footer" v-show="todos.length" v-cloak>
		<span class="todo-count">
			<strong>{{remaining}}</strong> {{remaining | pluralize}} left
		</span>
		<ul class="filters">
			<li><a href="#/all"
			:class="{selected: visibility === 'all'}">
			All</a></li>
			<li><a href="#/active"
			:class="{selected: visibility === 'active'}">
			Active</a></li>
			<li><a href="#/completed"
			:class="{selected: visibility === 'completed'}">
			Completed</a></li>
		</ul>
		<button class="clear-completed"
			v-show="todos.length > remaining"
			@click="removeCompleted">
			Clear completed
		</button>
	</footer>
</template>

<script>
export default {
	name: 'TodoController',
	filters: {
		pluralize(n) {
			return n === 1 ? 'item' : 'items';
		}
	},
	props: {
		todos: Array,
		remaining: Number,
		visibility: String
	},
	methods: {
		removeCompleted() {
			this.$emit('removeCompleted');
		}
	}
}
</script>
