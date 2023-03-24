<script setup>
import { defineProps, onMounted, ref, computed, watchEffect } from 'vue';
import { useTimer } from 'vue-timer-hook';
import images from '../assets/images';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
	onQuit: Function,
	shuffle: Boolean,
	time: Number,
	restart: Function,
})

const totalTime = ref(props.time);
const formattedTime = computed(() => `${timer.minutes.value < 10 ? '0' + timer.minutes.value : timer.minutes.value}:${timer.seconds.value < 10 ? '0' + timer.seconds.value : timer.seconds.value}`);	
const newTime = new Date();
newTime.setSeconds(newTime.getSeconds() + totalTime.value);
const timer = useTimer(newTime);
const imgs = computed(() => {
	if (props.shuffle) {
		return shuffle(images);
	} else {
		return images;
	}
});

let index = ref(0);
let isActive = ref(true);
let isPaused = ref(false);

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
	} else {
		isActive.value = true;
		index.value = imgs.value.length;
		displayPreviousImage();
	}
}

function onClickNext() {
	displayNextImage();
}

function displayPreviousImage() {
	if (isPaused.value) return;
	if (index.value < 1) {
		alert('This is the first image.')
		return;
	}
	restartTimer();
	console.log(timer.isRunning.value, timer.seconds.value);
	index.value--;
}

function displayNextImage() {
	if (isPaused.value) return;
	if (index.value + 1 > imgs.value.length - 1) {
		isActive.value = false;
		timer.restart(0);
		return;
	}
	restartTimer();
	console.log(timer.isRunning.value, timer.seconds.value);
	index.value++;
}

function playPause() {
	if (isActive.value) {
		if (!isPaused.value) {
			isPaused.value = !isPaused.value;
			timer.pause();
			console.log(timer.isRunning.value)
		} else {
			isPaused.value = !isPaused.value;
			timer.resume();
			console.log(timer.isRunning.value)
		}
	} else {
		return;
	}
}

function restartTimer() {
	const asd = new Date();
	asd.setSeconds(asd.getSeconds() + totalTime.value);
	timer.restart(asd);
}

onMounted(() => {
	console.log(totalTime.value);
	console.log(timer.isRunning.value);
	
	watchEffect(async () => {
		if(timer.isExpired.value) {
			console.warn('IsExpired');
			displayNextImage();
		}
	});
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
