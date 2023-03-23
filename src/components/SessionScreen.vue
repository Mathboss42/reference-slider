<script setup>
import { defineProps, onMounted, watch, ref, computed } from 'vue';
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
let isActive = true;

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

function displayNextImage() {
	if (index + 1 > imgs.value.length - 1) {
		isActive = false;
		return;
	}
	index++;
}

onMounted(() => {
	setTimeout(() => {
		totalTime.value--;
	}, 1000);
})

watch(totalTime, () => {
	if (isActive) {
		setTimeout(() => {
			totalTime.value--;
			if (totalTime.value < 0) {
				displayNextImage();
				totalTime.value = props.time;
			}
		}, 1000);
	} else {
		totalTime.value = 0;
	}
});
</script>

<template>
	<div class="session container">
		<div class="row justify-content-center align-items-center mb-3 pt-3">
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
		</div>
		<div class="row justify-content-center align-items-center mb-5">
			<div class="col-auto text-white fs-1 border">
				{{ formattedTime }}
			</div>
		</div>
		<div class="d-flex justify-content-center align-items-center">
			<img v-if="isActive" :src="imgs[index]" alt="user img">
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
