<script setup>
defineProps({
  number: { type: String, required: true },
  label: { type: String, required: true },
})
</script>

<template>
  <div class="section-label">
    <span class="section-label__number">{{ number }}</span>
    <span>{{ label }}</span>
  </div>
</template>
