<script setup>
import { defineProps } from 'vue';
import images from '../assets/images';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
	onQuit: Function,
})
</script>

<template>
	<div class="session container">
		<h1 class="text-white">Session</h1>
		<button
			@click="onQuit"
		>
			Quit
		</button>
	</div>
</template>

<style>

</style>
