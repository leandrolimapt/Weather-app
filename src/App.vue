<template>
  <div>
    <Weather/>
  </div>
</template>

<script>
import './assets/styles/main.css'
import Weather from "./components/Weather"
export default {
  name: 'App',
  components: {
    Weather
  }
}
</script>