<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const time = ref(60);
const router = useRouter()

function startSession() {
	console.log('start session');
	router.push({ name: 'session', params: { time: time.value}});
}

</script>

<template>
	<main>
		<div class="container">
			<div class="d-flex justify-content-center">
				<h1 class="h1 text-white">Reference Slider</h1>
			</div>
			<div class="row justify-content-center my-5">
				<div class="col-8">
					<div class="form-floating">
						<input type="email" class="form-control" id="floatingInput" placeholder="Pinterest Board URL">
						<label for="floatingInput" class="text-secondary">Pinterest Board URL</label>
					</div>
				</div>
				<div class="col-auto">
					<button
						@click="startSession"
						class="btn btn-primary btn-lg fs-4"
						id="start-button"
					>
						Start
					</button>
				</div>
			</div>
			<div class="row justify-content-center my-5">
				<div class="col-auto">
					<select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" id="display-time">
						<option value="0.5" selected>30s</option>
						<option value="1">1min</option>
						<option value="2">2min</option>
						<option value="3">3min</option>
						<option value="4">4min</option>
						<option value="5">5min</option>
						<option value="10">10min</option>
					</select>
				</div>
				<div class="col-auto align-self-center fs-4" id="time-label">
					<label for="display-time" class="text-white">Time per image</label>
				</div>
			</div>
			<div class="row justify-content-center my-5">
				<div class="form-check form-switch justify-content-center d-flex align-items-center">
					<input class="form-check-input big-checkbox me-2" type="checkbox" id="flexSwitchCheckDefault">
					<label class="form-check-label fs-4 text-white" for="flexSwitchCheckDefault">Shuffle Images</label>
				</div>
			</div>
		</div>
	</main>
</template>

<style scoped>
main {
	padding-top: 48px;
	display: flex;
	justify-content: center;
}

#flexSwitchCheckDefault {
	width: 50px;
	height: 25px;
}

#time-label {
	padding-left: 0;
}

#start-button {
	height: calc(3.5rem + 2px);
}
</style>
