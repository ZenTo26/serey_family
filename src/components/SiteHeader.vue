<script setup>
defineProps({
  soundOn: { type: Boolean, default: false },
})

defineEmits(['toggle-sound'])
</script>

<template>
  <header class="site-header">
    <a class="site-header__brand" href="#top">#𝑺𝑬𝑹𝑬𝒀 𝒙 𝑮𝑨𝑵𝑮</a>
    <span class="site-header__world">𝐖𝐄 𝐀𝐑𝐄 𝐒𝐄𝐑𝐄𝐘 𝐆𝐀𝐍𝐆</span>
    <button class="site-header__sound" type="button" @click="$emit('toggle-sound')">
      sound {{ soundOn ? 'on' : 'off' }}
    </button>
  </header>
</template>
