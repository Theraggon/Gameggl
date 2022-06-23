<script setup lang="ts">
import axios, { AxiosBasicCredentials } from 'axios';
import { ref } from 'vue';
import { api_key } from '../secrets';

const auth: AxiosBasicCredentials = {username: api_key, password: 'api_token'};
//const response = axios.get('https://api.track.toggl.com/api/v9/me', {auth});
//response.then((res) => console.log(res));
const timeToRefetchInMs = 60000;
const delay = (ms: number) => new Promise(r => setTimeout(r, ms));
const coins = ref(0);

do {
    const running = axios.get('https://api.track.toggl.com/api/v9/me/time_entries/current', {auth});
    running.then((res) => {
        console.log(res);
        if(res.data){
            coins.value++;
        }
        console.log('Coins:' + coins.value);
    });
    await delay(timeToRefetchInMs);
} while (true);




</script>

<template>
<div class="coins">
    Coins: {{ coins }}
</div>
</template>

<style>
.coins {
    font-size: 4rem;    
}
</style>
