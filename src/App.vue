<template>
  <div id="app">
    <DateInput v-model="dateInput" min-year="1900" />
  </div>
</template>

<script>
import DateInput from './components/DateInput.vue'

export default {
  name: 'App',
  data() {
    return {
      dateInput: null,
    }
  },
  components: {
    DateInput,
  },
  watch: {
    dateInput() {
      console.log('outside', this.dateInput)
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
