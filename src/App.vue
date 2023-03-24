<script setup>
import StartMenu from './components/StartMenu.vue';
import SessionScreen from './components/SessionScreen.vue';
import { ref, watch } from 'vue';
import defaultSettings from './settings';

const settings = ref(initSettings());
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
		settings.value.url = newURL;
		settings.value.isSessionStarted = true;
	} else if (settings.value.url) {
		settings.value.isSessionStarted = true;
	} else {
		alert('Please specify a URL to get images from.');
	}
}

function toggleShuffle() {
	settings.value.shuffle = !settings.value.shuffle;
}

function clearUrlInput() {
	settings.value.url = '';
}

function updateTime(e) {
	settings.value.time = Number(e.target.value)
}

function quit() {
	settings.value.isSessionStarted = false;
}

function onChangeInput(e) {
	settings.value.url = e.target.value;
}

function restart() {
	componentKey.value++;
}

function populateLocalStorage(settings) {
	for (let key in settings) {
		localStorage.setItem(key, JSON.stringify(settings[key]));
	}
}

function initSettings(settings = defaultSettings) {
	if (!localStorage.length > 0) {
		console.log('localstorage empty')
		populateLocalStorage(settings);
		return defaultSettings;
	} else {	
		let newSettings = defaultSettings;
		for (let key in settings) {
			newSettings[key] = JSON.parse(localStorage.getItem(key));
		}
		return newSettings;
	}
}

watch(settings.value, () => {
	populateLocalStorage(settings.value);
});

</script>

<template>
	<StartMenu 
		v-if="!settings.isSessionStarted" 
		:on-click-start="onClickStart" 
		:on-change-time="updateTime"
		:url="settings.url"
		:on-clear-url-input="clearUrlInput"
		:on-change-input="onChangeInput"
		:on-toggle-shuffle="toggleShuffle"
		:shuffle="settings.shuffle"
		:time="settings.time"
	/>
	<SessionScreen 
		v-else 
		:on-quit="quit"
		:shuffle="settings.shuffle"
		:time="settings.time"
		:restart="restart"
		:key="componentKey"
	/>
</template>

<style scoped>

</style>
