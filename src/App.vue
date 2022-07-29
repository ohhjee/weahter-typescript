<template>
<WeatherApp/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import WeatherApp from './components/WeatherApp.vue';

export default defineComponent({
  name: 'App',
  components: {
    WeatherApp
  }
});
</script>

<style>

</style>
