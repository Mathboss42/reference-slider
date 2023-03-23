<script setup>
import { defineProps, onMounted, ref, computed, onUnmounted } from 'vue';
import images from '../assets/images';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
	onQuit: Function,
	shuffle: Boolean,
	time: Number,
	restart: Function,
})

const totalTime = ref(props.time);
const minutes = computed(() => Math.floor(totalTime.value / 60));
const seconds = computed(() => totalTime.value - minutes.value * 60);
const formattedTime = computed(() => `${minutes.value < 10 ? '0' + minutes.value : minutes.value}:
						${seconds.value < 10 ? '0' + seconds.value : seconds.value}`);
const imgs = computed(() => {
	if (props.shuffle) {
		return shuffle(images);
	} else {
		return images;
	}
})

let index = 0;
let isActive = ref(true);
let isPaused = ref(false);
let interval;

function shuffle(array) {
  let currentIndex = array.length,  randomIndex;

  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]];
  }

  return array;
}

function onClickPrevious() {
	if (isActive.value) {
		displayPreviousImage()
		resetInterval();
	} else {
		isActive.value = true;
		index = imgs.value.length;
		displayPreviousImage();
		resetInterval();
	}
}

function onClickNext() {
	displayNextImage();
	resetInterval();
}

function displayPreviousImage() {
	if (index < 1) {
		alert('This is the first image.')
		return;
	}
	totalTime.value = props.time;
	index--;
}

function displayNextImage() {
	if (index + 1 > imgs.value.length - 1) {
		isActive.value = false;
		return;
	}
	totalTime.value = props.time;
	index++;
}

function resetInterval() {
	clearInterval(interval);
	if (isActive.value) {
		addInterval();
	} else {
		totalTime.value = 0;
	}
}

function playPause() {
	if (isActive.value) {
		if (!isPaused.value) {
			isPaused.value = !isPaused.value;
			clearInterval(interval);
		} else {
			isPaused.value = !isPaused.value;
			resetInterval();
		}
	} else {
		return;
	}
}

function addInterval() {
	interval = setInterval(() => {
		totalTime.value--;
		if (totalTime.value < 0) {
			displayNextImage()
		}
	}, 1000);
}

onMounted(() => {
	addInterval();
});

onUnmounted(() => {
	clearInterval(interval);
});
</script>

<template>
	<div class="session container">
		<div class="row justify-content-center align-items-center mb-3 pt-3">
			<div class="col-auto">
				<button
					class="btn btn-primary fs-4"
					@click="onClickPrevious"
				>
					&lt;
				</button>
			</div>
			<div class="col-auto">
				<button
					class="btn btn-primary fs-4"
					@click="playPause"
				>
					<i v-if="!isPaused" class="fa-solid fa-pause"></i>
					<i v-else class="fa-solid fa-play"></i>
				</button>
			</div>
			<div class="col-auto">
				<h1 class="text-white">Session</h1>
			</div>
			<div class="col-auto">
				<button
					class="btn btn-primary fs-4"
					@click="onQuit"
				>
					Quit
				</button>
			</div>
			<div class="col-auto">
				<button
					class="btn btn-primary fs-4"
					@click="onClickNext"
				>
					>
				</button>
			</div>
		</div>
		<div class="row justify-content-center align-items-center mb-5">
			<div class="col-auto text-white fs-1 border">
				{{ formattedTime }}
			</div>
		</div>
		<div class="d-flex justify-content-center align-items-center">
			<img v-if="isActive && !isPaused" :src="imgs[index]" alt="user img">
			<div v-else-if="isActive && isPaused" class="col-auto">
				<div class="row">
					<p class="text-white fs-1">Session Paused</p>
				</div>
				<div class="row">
					<div class="d-flex justify-content-center">
						<button class="btn btn-primary fs-4" @click="restart">Restart</button>
					</div>
				</div>
			</div>
			<div v-else class="col-auto">
				<div class="row">
					<p class="text-white fs-1">End of Session!</p>
				</div>
				<div class="row">
					<div class="d-flex justify-content-center">
						<button class="btn btn-primary fs-4" @click="restart">Restart</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<style>
img {
	max-height: 75vh;
	max-width: 100vw;
}
</style>
