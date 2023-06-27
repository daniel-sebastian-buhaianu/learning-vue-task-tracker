<template>
	<button 
		class="btn" 
		v-bind:style="{ backgroundColor: color }"
		@click="onClick()"
	>
		{{ text }}
	</button>
</template>

<script>
	export default {
		name: 'Button',
		props: {
			text: String,
			color: String,
		},
		methods: {
			onClick() {
				console.log('clicked')
			}
		}
	}
</script>