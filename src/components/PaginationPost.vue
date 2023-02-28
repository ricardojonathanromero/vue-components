<template>
  <div class="btn-group" role="group" aria-label="Basic example">
    <button
      type="button"
      class="btn btn-outline-primary"
      :disabled="disabledPrev"
      @click="emit('previousPage')"
    >Previous</button>
    <button
      type="button"
      class="btn btn-outline-primary"
      :disabled="disabledNext"
      @click="emit('nextPage')"
    >Next</button>
  </div>
</template>

<script setup>
defineProps({
  disabledPrev: {
    type: Boolean,
    default: false
  },
  disabledNext: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['previousPage', 'nextPage'])
</script>

<style scoped>

</style>
