<script setup>
defineProps({
  member: { type: Object, required: true },
  featured: { type: Boolean, default: false },
})
</script>

<template>
  <article class="roster-card" :class="{ 'roster-card--featured': featured }">
    <div class="roster-card__media">
      <img
        class="roster-card__image"
        :src="member.image"
        :alt="member.name"
        loading="lazy"
        draggable="false"
      />
      <span v-if="member.badge" class="roster-card__badge">{{ member.badge }}</span>
      <span class="roster-card__shade" aria-hidden="true"></span>
    </div>

    <div class="roster-card__footer">
      <span>{{ member.name }}</span>
    </div>
  </article>
</template>
