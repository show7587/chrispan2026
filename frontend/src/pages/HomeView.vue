<template>
  <div class="home-page">
    <!-- 1. Hero -->
    <section class="hero text-center">
      <v-container style="max-width: 960px;">
        <div class="d-flex flex-column align-center">
          <div class="avatar-wrapper mb-4">
            <v-avatar size="116" class="hero-avatar elevation-6">
              <img
                :src="assetUrl(profile.photo)"
                :alt="profile.name"
                class="avatar-img"
              >
            </v-avatar>
            <div class="number-badge elevation-4" aria-label="號次">
              {{ profile.number }}
            </div>
          </div>

          <div class="hero-eyebrow">{{ profile.title }}</div>

          <div class="d-flex align-center justify-center ga-3 my-1">
            <span class="number-circle">{{ profile.number }}</span>
            <h1 class="hero-name">{{ profile.name }}</h1>
          </div>

          <div class="hero-tagline mt-1">{{ profile.tagline }}</div>
          <div class="hero-slogan mt-2">{{ profile.slogan }}</div>

          <p v-if="profile.intro" class="hero-intro mt-4">
            {{ profile.intro }}
          </p>
        </div>

        <!-- 三大願景卡 (pillars) -->
        <v-row dense class="mt-6">
          <v-col
            v-for="pillar in profile.pillars"
            :key="pillar.title"
            cols="12"
            sm="4"
          >
            <v-card class="pillar-card h-100 text-center py-4 px-3" elevation="2">
              <div class="pillar-title">{{ pillar.title }}</div>
              <div class="pillar-desc mt-1">{{ pillar.desc }}</div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- 2. 政見主張 -->
    <v-container id="platform" class="py-8" style="max-width: 960px;">
      <div class="section-header text-center mb-6">
        <div class="section-kicker">政見主張</div>
        <h2 class="section-title">{{ profile.tagline }}</h2>
      </div>

      <v-row>
        <v-col
          v-for="(group, idx) in platform"
          :key="group.key"
          cols="12"
          md="6"
        >
          <v-card class="platform-card h-100 d-flex flex-column" elevation="2">
            <div
              class="platform-header d-flex align-center ga-3 px-4 py-3"
              :style="{ borderTopColor: group.color }"
            >
              <div
                class="platform-seq"
                :style="{ backgroundColor: group.color }"
              >
                {{ formatSeq(idx + 1) }}
              </div>
              <div class="flex-grow-1 min-w-0">
                <div class="d-flex align-center ga-2">
                  <v-icon :icon="group.icon" :color="group.color" size="22" />
                  <h3 class="platform-title">{{ group.title }}</h3>
                </div>
                <div class="platform-subtitle">{{ group.subtitle }}</div>
              </div>
            </div>

            <!-- 3 張照片縮圖（小圖，不放大超過 2 倍） -->
            <div class="platform-thumbs px-4 pt-2 pb-1">
              <div
                v-for="img in group.images"
                :key="img.src"
                class="thumb-box"
              >
                <img
                  :src="assetUrl(img.src)"
                  :alt="img.alt"
                  loading="lazy"
                  class="thumb-img"
                >
              </div>
            </div>

            <v-card-text class="pt-3 pb-4 px-4 flex-grow-1">
              <ul class="platform-list">
                <li v-for="item in group.items" :key="item">
                  {{ item }}
                </li>
              </ul>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <!-- 3. 政績摘要 -->
    <section class="achievements-summary py-8">
      <v-container style="max-width: 960px;">
        <div class="text-center mb-4">
          <div class="tenure-badge">{{ profile.tenure }}</div>
          <h2 class="section-title mt-2">{{ profile.achievementsHeading }}</h2>
        </div>

        <!-- honor 橫幅 -->
        <v-card class="honor-banner text-center py-3 px-4 mb-6" elevation="2">
          <div class="d-flex align-center justify-center flex-wrap ga-2">
            <v-icon icon="mdi-trophy-award" color="#FFF4D2" size="24" />
            <span class="honor-text">{{ profile.honor }}</span>
          </div>
        </v-card>

        <!-- 8 項 stats：手機 2 欄、桌機 4 欄 -->
        <v-row dense>
          <v-col
            v-for="stat in achievements.stats"
            :key="stat.label"
            cols="6"
            md="3"
          >
            <v-card class="stat-card text-center py-4 px-2 h-100" elevation="1">
              <div class="stat-number">
                <span class="stat-value">{{ stat.value }}</span>
                <span class="stat-unit">{{ stat.unit }}</span>
              </div>
              <div class="stat-label mt-1">{{ stat.label }}</div>
            </v-card>
          </v-col>
        </v-row>

        <div class="text-center mt-7">
          <v-btn
            to="/achievements"
            color="primary"
            variant="flat"
            size="x-large"
            rounded="pill"
            append-icon="mdi-chevron-right"
            class="more-btn px-8"
          >
            看完整政績
          </v-btn>
        </div>
      </v-container>
    </section>

    <!-- Footer -->
    <footer class="footer text-center py-6">
      <div class="footer-title">{{ profile.title }} {{ profile.name }}</div>
      <div class="footer-sub mt-1">{{ profile.slogan }}｜{{ profile.tagline }}</div>
    </footer>
  </div>
</template>

<script setup>
import profile from '@/data/profile.json'
import platform from '@/data/platform.json'
import achievements from '@/data/achievements.json'

const assetUrl = (src) => `${import.meta.env.BASE_URL}${src}`
const formatSeq = (n) => String(n).padStart(2, '0')
</script>

<style scoped>
.home-page {
  min-height: 100vh;
  overflow-x: hidden;
  background-color: #FAF7F0;
}

.hero {
  background: linear-gradient(155deg, #163d27 0%, #1F5135 58%, #286342 100%);
  color: #ffffff;
  padding: 40px 12px 36px;
  border-bottom: 4px solid #C9A45C;
}

.avatar-wrapper {
  position: relative;
  display: inline-block;
}

.hero-avatar {
  border: 4px solid #C9A45C;
  background: #ffffff;
  overflow: hidden;
}

.avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.number-badge {
  position: absolute;
  right: -4px;
  bottom: -4px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: #C9A45C;
  color: #163d27;
  font-weight: 900;
  font-size: 1.15rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid #ffffff;
}

.hero-eyebrow {
  font-size: 1.05rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  color: #f3dfb2;
}

.number-circle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: #C9A45C;
  color: #163d27;
  font-size: 1.5rem;
  font-weight: 900;
  border: 2px solid #fff6df;
  flex-shrink: 0;
}

.hero-name {
  font-size: clamp(2.25rem, 7vw, 3.25rem);
  font-weight: 900;
  letter-spacing: 0.06em;
  margin: 0;
  line-height: 1.15;
  color: #ffffff;
}

.hero-tagline {
  font-size: clamp(1.25rem, 4vw, 1.6rem);
  font-weight: 800;
  color: #ffe7b0;
  letter-spacing: 0.05em;
}

.hero-slogan {
  display: inline-block;
  padding: 4px 16px;
  border-radius: 999px;
  background: rgba(201, 164, 92, 0.22);
  border: 1px solid rgba(201, 164, 92, 0.65);
  color: #fff8e6;
  font-weight: 700;
  font-size: 0.98rem;
  letter-spacing: 0.06em;
}

.hero-intro {
  max-width: 680px;
  font-size: 0.96rem;
  line-height: 1.7;
  color: rgba(255, 255, 255, 0.92);
  margin: 0 auto;
}

.pillar-card {
  border-radius: 14px;
  background: #ffffff;
  border-top: 4px solid #C9A45C;
}

.pillar-title {
  font-size: 1.15rem;
  font-weight: 800;
  color: #1F5135;
  letter-spacing: 0.04em;
}

.pillar-desc {
  font-size: 0.92rem;
  font-weight: 600;
  color: #4a5a50;
  line-height: 1.45;
}

.section-kicker {
  font-size: 0.88rem;
  font-weight: 800;
  letter-spacing: 0.14em;
  color: #9e782f;
}

.section-title {
  font-size: clamp(1.5rem, 4.5vw, 1.9rem);
  font-weight: 800;
  color: #1F5135;
  margin: 2px 0 0;
}

.platform-card {
  border-radius: 16px;
  background: #ffffff;
  border: 1px solid rgba(31, 81, 53, 0.12);
  overflow: hidden;
}

.platform-header {
  border-top: 4px solid #1F5135;
  background: linear-gradient(180deg, rgba(250, 247, 240, 0.9) 0%, #ffffff 100%);
}

.platform-seq {
  width: 42px;
  height: 42px;
  border-radius: 10px;
  color: #ffffff;
  font-weight: 900;
  font-size: 1.15rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  letter-spacing: 0.02em;
}

.min-w-0 {
  min-width: 0;
}

.platform-title {
  font-size: 1.25rem;
  font-weight: 800;
  color: #1F5135;
  margin: 0;
  line-height: 1.3;
}

.platform-subtitle {
  font-size: 0.9rem;
  font-weight: 600;
  color: #5b695f;
  margin-top: 2px;
  line-height: 1.4;
}

.platform-thumbs {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 140px));
  gap: 8px;
  justify-content: start;
}

.thumb-box {
  width: 100%;
  max-width: 140px;
  height: 68px;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid rgba(31, 81, 53, 0.16);
  background: #f0ece1;
}

.thumb-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.platform-list {
  margin: 0;
  padding-left: 1.15rem;
  color: #26352c;
}

.platform-list li {
  margin-bottom: 6px;
  line-height: 1.55;
  font-weight: 500;
}

.achievements-summary {
  background: linear-gradient(180deg, #f3ede0 0%, #FAF7F0 100%);
  border-top: 1px solid rgba(201, 164, 92, 0.35);
}

.tenure-badge {
  display: inline-block;
  padding: 4px 16px;
  border-radius: 999px;
  background: #1F5135;
  color: #ffe7b0;
  font-weight: 800;
  font-size: 0.92rem;
  letter-spacing: 0.06em;
}

.honor-banner {
  border-radius: 14px;
  background: linear-gradient(135deg, #1F5135 0%, #2a6845 100%);
  border: 2px solid #C9A45C;
  color: #ffffff;
}

.honor-text {
  font-size: 1.05rem;
  font-weight: 800;
  color: #fff6df;
  letter-spacing: 0.03em;
}

.stat-card {
  border-radius: 14px;
  background: #ffffff;
  border: 1px solid rgba(201, 164, 92, 0.45);
}

.stat-number {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 3px;
}

.stat-value {
  font-size: clamp(1.75rem, 5vw, 2.2rem);
  font-weight: 900;
  color: #1F5135;
  line-height: 1.15;
}

.stat-unit {
  font-size: 0.95rem;
  font-weight: 800;
  color: #9e782f;
}

.stat-label {
  font-size: 0.9rem;
  font-weight: 700;
  color: #3f4f45;
}

.more-btn {
  font-weight: 800;
  letter-spacing: 0.04em;
  border: 2px solid #C9A45C;
}

.footer {
  background: #163d27;
  color: #e6dfd0;
  border-top: 3px solid #C9A45C;
}

.footer-title {
  font-weight: 800;
  font-size: 1rem;
  color: #ffffff;
}

.footer-sub {
  font-size: 0.85rem;
  color: #d8c9a8;
}
</style>
