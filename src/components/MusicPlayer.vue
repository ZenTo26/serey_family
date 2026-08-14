<script setup>
import { computed, nextTick, onBeforeUnmount, onMounted, ref, watch } from 'vue'

const props = defineProps({
  source: { type: String, required: true },
  cover: { type: String, required: true },
  title: { type: String, required: true },
  artist: { type: String, required: true },
  entered: { type: Boolean, default: false },
  soundOn: { type: Boolean, default: true },
})

const emit = defineEmits(['sound-change'])

const audio = ref(null)
const isPlaying = ref(false)
const current = ref(0)
const duration = ref(0)
const seeking = ref(false)

const progress = computed(() => {
  if (!duration.value) return 0
  return (current.value / duration.value) * 100
})

function formatTime(value) {
  if (!Number.isFinite(value) || value < 0) return '0:00'
  const min = Math.floor(value / 60)
  const sec = Math.floor(value % 60).toString().padStart(2, '0')
  return `${min}:${sec}`
}

async function play() {
  if (!audio.value) return
  try {
    await audio.value.play()
    isPlaying.value = true
  } catch {
    isPlaying.value = false
  }
}

function pause() {
  audio.value?.pause()
  isPlaying.value = false
}

function togglePlay() {
  if (!audio.value) return
  audio.value.paused ? play() : pause()
}

function toggleMute() {
  if (!audio.value) return
  audio.value.muted = !audio.value.muted
  emit('sound-change', !audio.value.muted)
}

function updateTime() {
  if (!audio.value || seeking.value) return
  current.value = audio.value.currentTime || 0
}

function updateDuration() {
  if (!audio.value) return
  duration.value = Number.isFinite(audio.value.duration) ? audio.value.duration : 0
}

function seek(event) {
  if (!audio.value || !duration.value) return
  const percentage = Number(event.target.value)
  current.value = (percentage / 100) * duration.value
  audio.value.currentTime = current.value
}

watch(
  () => props.entered,
  async (value) => {
    if (!value) return
    await nextTick()
    // The site-entry click is a user gesture, so this is the correct moment to request playback.
    play()
  },
)

watch(
  () => props.soundOn,
  (value) => {
    if (audio.value) audio.value.muted = !value
  },
)

onMounted(() => {
  if (audio.value) audio.value.muted = !props.soundOn
})

onBeforeUnmount(() => pause())
</script>

<template>
  <aside class="music-player" aria-label="Music player">
    <img class="music-player__cover" :src="cover" alt="Album cover" draggable="false" />

    <div class="music-player__meta">
      <strong>{{ title }}</strong>
      <span>{{ artist }}</span>
    </div>

    <button class="music-player__play" type="button" :aria-label="isPlaying ? 'Pause' : 'Play'" @click="togglePlay">
      <span v-if="!isPlaying">▶</span>
      <span v-else class="pause-icon">Ⅱ</span>
    </button>

    <div class="music-player__timeline">
      <div class="music-player__times">
        <span>{{ formatTime(current) }}</span>
        <span>/</span>
        <span>{{ formatTime(duration) }}</span>
      </div>

      <input
        class="music-player__range"
        type="range"
        min="0"
        max="100"
        step="0.1"
        :value="progress"
        aria-label="Song progress"
        @pointerdown="seeking = true"
        @pointerup="seeking = false"
        @input="seek"
      />
    </div>

    <button class="music-player__volume" type="button" @click="toggleMute">
      {{ soundOn ? 'VOL' : 'MUTE' }}
    </button>

    <audio
      ref="audio"
      preload="metadata"
      :src="source"
      @timeupdate="updateTime"
      @loadedmetadata="updateDuration"
      @durationchange="updateDuration"
      @ended="isPlaying = false"
      @play="isPlaying = true"
      @pause="isPlaying = false"
    ></audio>
  </aside>
</template>
