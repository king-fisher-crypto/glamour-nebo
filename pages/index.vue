<template>
  <div id="app">
    <Quiz class="quiz-container" />
  </div>
</template>

<script>
import Quiz from '../components/Quiz.vue';

export default {
  name: 'App',
  components: {
    Quiz,
  },
  scrollToTop: true
}
</script>
