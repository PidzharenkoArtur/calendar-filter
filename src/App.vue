<template>
  <div>
    <CalendarFilter/>
  </div>
</template>

<script>
import CalendarFilter from './components/CalendarFilter'

export default {
  name: 'App',
  components: {
    CalendarFilter
  }
}
</script>

<style lang="scss">
@import './assets/sass/normalize';
@import url('https://fonts.googleapis.com/css2?family=PT+Sans&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap');
button {
  background: transparent;
  outline: none;
  border: none;
  cursor: pointer;
}
</style>
