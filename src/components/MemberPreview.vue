<script setup>
defineProps({
  member: {
    type: Object,
    default: null,
  },
})
</script>

<template>
  <div class="member-preview" :class="{ 'member-preview--active': member }" aria-hidden="true">
    <Transition name="member-image" mode="out-in">
      <figure v-if="member" :key="member.image" class="member-preview__figure">
        <img class="member-preview__image" :src="member.image" :alt="member.name" />
        <figcaption class="member-preview__caption">
          <span>{{ member.name }}</span>
          <span>WEH WORLDWIDE</span>
        </figcaption>
      </figure>
    </Transition>
  </div>
</template>
