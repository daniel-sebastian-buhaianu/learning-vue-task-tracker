<template>
	<header>
		<h1>{{ title }}</h1>
		<Button 
			v-bind:text="showAddTask ? 'Close' : 'Add Task'"
			v-bind:color="showAddTask ? 'red' : 'green'" 
			@click="$emit('toggle-add-task')" 
		/>
	</header>
</template>

<script>
	import Button from './Button'

	export default {
		name: 'Header',
		props: {
			title: String,
			showAddTask: Boolean,
		},
		components: {
			Button,
		}
	}
</script>

<style scoped>
	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 20px;
	}
</style>