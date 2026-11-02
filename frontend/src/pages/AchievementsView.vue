<template>
  <div class="achievements-page">
    <!-- 1. 標題 tenure + achievementsHeading；學歷、經歷兩欄卡片 -->
    <section class="page-header text-center">
      <v-container style="max-width: 960px;">
        <div class="tenure-pill">{{ profile.tenure }}</div>
        <h1 class="page-title mt-2">{{ profile.achievementsHeading }}</h1>
        <div class="honor-sub mt-3">
          <v-icon icon="mdi-trophy-award" color="#FFE7B0" size="20" class="mr-1" />
          <span>{{ profile.honor }}</span>
        </div>

        <v-row dense class="mt-5 text-left">
          <v-col cols="12" md="6">
            <v-card class="bio-card h-100 pa-4" elevation="2">
              <div class="bio-card-title d-flex align-center ga-2 mb-2">
                <v-icon icon="mdi-school" color="primary" size="22" />
                <span>學歷</span>
              </div>
              <ul class="bio-list">
                <li v-for="edu in profile.education" :key="edu">
                  {{ edu }}
                </li>
              </ul>
            </v-card>
          </v-col>

          <v-col cols="12" md="6">
            <v-card class="bio-card h-100 pa-4" elevation="2">
              <div class="bio-card-title d-flex align-center ga-2 mb-2">
                <v-icon icon="mdi-briefcase-check" color="primary" size="22" />
                <span>經歷</span>
              </div>
              <ul class="bio-list">
                <li v-for="exp in profile.experience" :key="exp">
                  {{ exp }}
                </li>
              </ul>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- 2. 分類篩選（全部／建設／福利／文化） -->
    <div class="filter-bar">
      <v-container class="py-1 px-2" style="max-width: 960px;">
        <v-chip-group
          v-model="selectedCategory"
          mandatory
          selected-class="filter-chip-active"
          class="justify-center"
        >
          <v-chip value="all" class="filter-chip" variant="flat">
            全部
          </v-chip>
          <v-chip
            v-for="cat in achievements.categories"
            :key="cat.key"
            :value="cat.key"
            :prepend-icon="cat.icon"
            class="filter-chip"
            variant="flat"
          >
            {{ cat.label }}
          </v-chip>
        </v-chip-group>
      </v-container>
    </div>

    <!-- 3. 每個分類一個區塊：4 張照片卡（圖＋caption）與 items 條列（帶 check icon） -->
    <v-container class="py-6" style="max-width: 960px;">
      <div
        v-for="cat in filteredCategories"
        :key="cat.key"
        class="category-section mb-8"
      >
        <v-card class="category-card pa-4 pa-sm-6" elevation="2">
          <div class="d-flex align-center ga-3 mb-4 pb-3 category-header">
            <v-avatar :color="cat.color" size="42">
              <v-icon :icon="cat.icon" color="white" size="22" />
            </v-avatar>
            <h2 class="category-title">{{ cat.label }}</h2>
          </div>

          <!-- 4 張照片小卡片（不放大超過約 2 倍） -->
          <div class="photos-grid mb-5">
            <div
              v-for="photo in cat.photos"
              :key="photo.src"
              class="photo-mini-card"
            >
              <div class="photo-thumb-wrap">
                <img
                  :src="assetUrl(photo.src)"
                  :alt="photo.caption"
                  loading="lazy"
                  class="photo-thumb"
                >
              </div>
              <div class="photo-caption">{{ photo.caption }}</div>
            </div>
          </div>

          <!-- items 條列（帶 check icon） -->
          <v-row dense>
            <v-col
              v-for="item in cat.items"
              :key="item"
              cols="12"
              sm="6"
            >
              <div class="achievement-item d-flex align-start ga-2 py-1">
                <v-icon
                  icon="mdi-check-circle"
                  :color="cat.color"
                  size="20"
                  class="mt-1 flex-shrink-0"
                />
                <span class="achievement-text">{{ item }}</span>
              </div>
            </v-col>
          </v-row>
        </v-card>
      </div>

      <div class="text-center mt-6 mb-4">
        <v-btn
          to="/"
          color="primary"
          variant="flat"
          size="large"
          rounded="pill"
          prepend-icon="mdi-arrow-left"
          class="back-btn px-6"
        >
          回首頁看政見
        </v-btn>
      </div>
    </v-container>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import profile from '@/data/profile.json'
import achievements from '@/data/achievements.json'

const selectedCategory = ref('all')

const filteredCategories = computed(() =>
  selectedCategory.value === 'all'
    ? achievements.categories
    : achievements.categories.filter(c => c.key === selectedCategory.value),
)

const assetUrl = (src) => `${import.meta.env.BASE_URL}${src}`
</script>

<style scoped>
.achievements-page {
  min-height: 100vh;
  overflow-x: hidden;
  background-color: #FAF7F0;
}

.page-header {
  background: linear-gradient(155deg, #163d27 0%, #1F5135 58%, #286342 100%);
  color: #ffffff;
  padding: 32px 12px 28px;
  border-bottom: 4px solid #C9A45C;
}

.tenure-pill {
  display: inline-block;
  padding: 4px 16px;
  border-radius: 999px;
  background: rgba(201, 164, 92, 0.25);
  border: 1px solid #C9A45C;
  color: #ffe7b0;
  font-weight: 800;
  font-size: 0.92rem;
  letter-spacing: 0.06em;
}

.page-title {
  font-size: clamp(1.75rem, 5vw, 2.3rem);
  font-weight: 900;
  color: #ffffff;
  margin: 0;
}

.honor-sub {
  font-size: 0.95rem;
  font-weight: 700;
  color: #fff4d2;
}

.bio-card {
  border-radius: 14px;
  background: #ffffff;
  border-top: 4px solid #C9A45C;
}

.bio-card-title {
  font-size: 1.1rem;
  font-weight: 800;
  color: #1F5135;
}

.bio-list {
  margin: 0;
  padding-left: 1.15rem;
  color: #26352c;
}

.bio-list li {
  margin-bottom: 4px;
  line-height: 1.55;
  font-weight: 500;
}

.filter-bar {
  position: sticky;
  top: 48px;
  z-index: 2;
  background: rgba(250, 247, 240, 0.96);
  border-bottom: 1px solid rgba(31, 81, 53, 0.14);
  box-shadow: 0 4px 12px rgba(31, 81, 53, 0.08);
}

.filter-chip {
  min-height: 38px;
  font-weight: 700;
  background: #ffffff;
  color: #1F5135;
  border: 1px solid rgba(31, 81, 53, 0.22);
}

.filter-chip-active {
  background: #1F5135 !important;
  color: #ffffff !important;
  border-color: #1F5135 !important;
}

.category-card {
  border-radius: 16px;
  background: #ffffff;
  border: 1px solid rgba(31, 81, 53, 0.12);
}

.category-header {
  border-bottom: 2px solid rgba(201, 164, 92, 0.4);
}

.category-title {
  font-size: 1.4rem;
  font-weight: 800;
  color: #1F5135;
  margin: 0;
}

.photos-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 180px));
  gap: 10px;
  justify-content: start;
}

@media (min-width: 600px) {
  .photos-grid {
    grid-template-columns: repeat(4, minmax(0, 180px));
    gap: 12px;
  }
}

.photo-mini-card {
  max-width: 180px;
  border-radius: 10px;
  overflow: hidden;
  background: #FAF7F0;
  border: 1px solid rgba(31, 81, 53, 0.16);
  display: flex;
  flex-direction: column;
}

.photo-thumb-wrap {
  width: 100%;
  height: 78px;
  background: #e9e4d8;
  overflow: hidden;
}

.photo-thumb {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.photo-caption {
  padding: 6px 8px;
  font-size: 0.82rem;
  font-weight: 700;
  color: #1F5135;
  line-height: 1.35;
  text-align: center;
}

.achievement-item {
  line-height: 1.5;
}

.achievement-text {
  font-size: 0.96rem;
  font-weight: 600;
  color: #26352c;
}

.back-btn {
  font-weight: 800;
  border: 2px solid #C9A45C;
}
</style>
