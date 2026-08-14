<script setup>
defineProps({
  graphic: { type: String, required: true },
})

defineEmits(['enter'])
</script>

<template>
  <button class="intro" type="button" aria-label="Enter website" @click="$emit('enter')">
    <!-- <img class="intro__graphic" :src="graphic" alt="WEH" draggable="false" /> -->

    <p class="intro__brand">#𝑺𝑬𝑹𝑬𝒀 𝒙 𝑮𝑨𝑵𝑮</p>
    <p class="intro__prompt">— tap anywhere to enter —</p>
  </button>
</template>
