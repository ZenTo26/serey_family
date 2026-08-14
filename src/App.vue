<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import IntroOverlay from './components/IntroOverlay.vue'
import MemberCard from './components/MemberCard.vue'
import MusicPlayer from './components/MusicPlayer.vue'
import SectionLabel from './components/SectionLabel.vue'
import SiteHeader from './components/SiteHeader.vue'
import { brand, gxds, links, media, members, track } from './siteConfig'

const entered = ref(false)
const soundOn = ref(true)

const gxdCount = computed(() => String(gxds.length).padStart(2, '0'))
const memberCount = computed(() => String(members.length).padStart(2, '0'))

function enterSite() {
  entered.value = true
  document.documentElement.classList.remove('is-locked')
}

function setSound(value) {
  soundOn.value = value
}

function toggleSound() {
  soundOn.value = !soundOn.value
}

function onKeydown(event) {
  if (!entered.value && (event.key === 'Enter' || event.key === ' ')) {
    event.preventDefault()
    enterSite()
  }
}

onMounted(() => {
  if (!entered.value) document.documentElement.classList.add('is-locked')
  window.addEventListener('keydown', onKeydown)
})

onUnmounted(() => {
  document.documentElement.classList.remove('is-locked')
  window.removeEventListener('keydown', onKeydown)
})
</script>

<template>
  <Transition name="intro-leave">
    <IntroOverlay v-if="!entered" :graphic="media.introGraphic" @enter="enterSite" />
  </Transition>

  <div class="page" :class="{ 'page--entered': entered }">
    <SiteHeader :sound-on="soundOn" @toggle-sound="toggleSound" />

    <main>
      <HeroSection />

      <!-- Only this roster area uses the dark member-card art direction. -->
      <div class="roster-zone">
        <div
          class="roster-zone__background"
          :style="{ backgroundImage: `url(${media.rosterBackground})` }"
          aria-hidden="true"
        ></div>
        <div class="roster-zone__vignette" aria-hidden="true"></div>
        <div class="roster-zone__scanlines" aria-hidden="true"></div>
        <div class="roster-zone__noise" aria-hidden="true"></div>

        <section id="gxds" class="roster-section roster-section--gxds">
          <header class="roster-heading">
            <h2>BOSS</h2>
            <p><strong>{{ gxdCount }}</strong> </p>
          </header>

          <div class="gxds-card-grid">
            <MemberCard
              v-for="member in gxds"
              :key="member.name"
              :member="member"
              featured
            />
          </div>

          <div class="roster-divider" aria-hidden="true">
            
            <span>MEMBERS</span>
          </div>
        </section>

        <section id="members" class="roster-section roster-section--members">
          <header class="roster-heading roster-heading--members">
            <h2>SEREY MEMBER</h2>
            <p><strong>{{ memberCount }}</strong> members</p>
          </header>

          <div class="members-card-grid">
            <MemberCard
              v-for="member in members"
              :key="member.name"
              :member="member"
            />
          </div>
        </section>
      </div>

      <!-- <section id="join" class="feature-section feature-section--join">
        <SectionLabel number="03" label="JOIN" />

        <div class="feature-section__body">
          <p class="feature-section__small">Weh X Richman Official Server</p>

          <a class="feature-link" :href="links.join" target="_blank" rel="noreferrer">
            <span>#richmanxweh worldwide</span>
            <span class="feature-link__aside">
              <small>Join the Discord server</small>
              <span aria-hidden="true">→</span>
            </span>
          </a>
        </div>
      </section>

      <section id="collabs" class="feature-section feature-section--collabs">
        <SectionLabel number="04" label="COLLABS" />

        <div class="feature-section__body collabs">
          <p class="feature-section__small">Wxrldwide Collab</p>

          <div class="collabs__list">
            <a
              v-for="collab in links.collabs"
              :key="collab.label"
              class="collab-link"
              :href="collab.href"
              target="_blank"
              rel="noreferrer"
            >
              <span>{{ collab.label }}</span>
              <span aria-hidden="true">↗</span>
            </a>
          </div>
        </div>
      </section> -->
    </main>

    <footer class="footer">
      <h2>{{ brand.name }}</h2>
      <div class="footer__meta">
        <span>© SereyFamily 2026</span>
        <span>managed &amp; developed by Serey KimTo</span>
      </div>
    </footer>

    <MusicPlayer
      :source="media.audioUrl"
      :cover="media.albumCover"
      :title="track.title"
      :artist="track.artist"
      :entered="entered"
      :sound-on="soundOn"
      @sound-change="setSound"
    />
  </div>
</template>
