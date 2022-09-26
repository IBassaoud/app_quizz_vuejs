<template>
  <div class="quizz_completed">
    <div class="quizz_completed_content">
      <p class="quizz_completed_item">The Quizz is over !</p>
      <p class="quizz_completed_item">{{ percent }}% Score</p>

      <!--  BUTTONS -->
      <div class="quizz_completed_button">
        <!-- <div class="quizz_completed_button_item">Done</div> -->
        <div class="quizz_completed_button_item" @click="$emit('restartQuizz')">Retry</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["percent"],
  setup() {},
};
</script>
