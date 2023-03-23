<script setup>
import StartMenu from './components/StartMenu.vue';
import SessionScreen from './components/SessionScreen.vue';
import { ref } from 'vue';


const isSessionStarted = ref(false);
const time = ref(30);
const URL = ref(null);
const shuffle = ref(false);
const componentKey = ref(0);


function onClickStart(e) {
	const formData = new FormData(e.target)
    let newURL;
    // eslint-disable-next-line no-unused-vars
    for (const [name, value] of formData) {
        newURL = value;
    }
	e.target.reset();

	if (newURL) {
		URL.value = newURL;
		isSessionStarted.value = true;
	} else if (URL.value) {
		isSessionStarted.value = true;
	} else {
		alert('Please specify a URL to get images from.');
	}
}

function toggleShuffle() {
	shuffle.value = !shuffle.value;
}

function clearUrlInput() {
	URL.value = '';
}

function updateTime(e) {
	time.value = Number(e.target.value)
}

function quit() {
	isSessionStarted.value = false;
}

function onChangeInput(e) {
	URL.value = e.target.value;
}

function restart() {
	componentKey.value++;
}
</script>

<template>
	<StartMenu 
		v-if="!isSessionStarted" 
		:on-click-start="onClickStart" 
		:on-change-time="updateTime"
		:url="URL"
		:on-clear-url-input="clearUrlInput"
		:on-change-input="onChangeInput"
		:on-toggle-shuffle="toggleShuffle"
		:shuffle="shuffle"
		:time="time"
	/>
	<SessionScreen 
		v-else 
		:on-quit="quit"
		:shuffle="shuffle"
		:time="time"
		:restart="restart"
		:key="componentKey"
	/>
</template>

<style scoped>

</style>
