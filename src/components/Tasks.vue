<template>
	<div 
		v-for="task in tasks" 
		v-bind:id="task.id"
	>
		<Task 
			v-bind:task="task" 
			@delete-task="$emit('delete-task', task.id)" 
			@toggle-reminder="$emit('toggle-reminder', task.id)"
		/>
	</div>
</template>

<script>
	import Task from './Task'

	export default {
		name: 'Tasks',
		components: {
			Task,
		},
		props: {
			tasks: Array,
		},
		emits: ['delete-task', 'toggle-reminder']
	}
</script>