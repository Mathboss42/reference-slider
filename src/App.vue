<script setup>
import StartMenu from './components/StartMenu.vue';
import SessionScreen from './components/SessionScreen.vue';
import { ref } from 'vue';


const isSessionStarted = ref(false);
const time = ref(0.5);
const URL = ref(null);


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

function clearUrlInput() {
	console.log('asd')
	URL.value = '';
}

function updateTime(e) {
	console.log(time.value)
	time.value = Number(e.target.value)
	console.log(time.value)
}

function quit() {
	isSessionStarted.value = false;
}

function onChangeInput(e) {
	URL.value = e.target.value;
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
	/>
	<SessionScreen 
		v-else 
		:on-quit="quit"
	/>
</template>

<style scoped>

</style>
