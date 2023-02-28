<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{ id }} - {{ title }}</h5>
      <p>{{ body }}</p>
      <button class="btn btn-primary" @click="emit('changeFav', {id, title, body})">favorite</button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  id: Number,
  title: String,
  body: {
    type: String,
    default: 'no description'
  }
});

const emit = defineEmits(['changeFav']);
</script>

<style scoped>

</style>
