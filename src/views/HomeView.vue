<script setup>
import { ref } from 'vue';
import router from '.././router';

const time = ref(60);

function startSession() {
    console.log('start session');
    router.push({ name: 'session', params: { time: time.value}});
}

</script>

<template>
  <main>
    <button 
        @click="startSession"
        class="btn btn-primary"
    >
        Start
    </button>
  </main>
</template>

<style scoped>
main {
    padding-top: 48px;
    display: flex;
    justify-content: center;
}
</style>
