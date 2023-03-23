<script setup>
import StartMenu from './components/StartMenu.vue';
import SessionScreen from './components/SessionScreen.vue';
import { ref } from 'vue';


const isSessionStarted = ref(false);
const time = ref(0.5);


function onClickStart() {
	isSessionStarted.value = true;
	console.log('ghell');
}

function updateTime(e) {
	console.log(time.value)
	time.value = Number(e.target.value)
	console.log(time.value)
}

function quit() {
	isSessionStarted.value = false;
}
</script>

<template>
	<StartMenu v-if="!isSessionStarted" :on-click-start="onClickStart" :on-change-time="updateTime"/>
	<SessionScreen v-else :on-quit="quit"/>
</template>

<style scoped>

</style>
