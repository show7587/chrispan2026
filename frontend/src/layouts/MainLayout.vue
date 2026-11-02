<template>
  <v-app-bar density="compact" color="primary" elevation="2">
    <v-app-bar-title class="app-bar-title">
      <router-link to="/" class="app-bar-link">
        萬金村村長候選人 潘譽仁
      </router-link>
    </v-app-bar-title>
    <template #append>
      <div class="d-flex align-center ga-1 pr-1">
        <v-btn
          to="/"
          variant="text"
          size="small"
          class="nav-btn"
        >
          政見
        </v-btn>
        <v-btn
          to="/achievements"
          variant="text"
          size="small"
          class="nav-btn"
        >
          政績
        </v-btn>
      </div>
    </template>
  </v-app-bar>
  <router-view />
</template>

<script setup>
</script>

<style scoped>
.app-bar-title {
  color: #ffffff;
  font-weight: 800;
  font-size: 1rem;
  letter-spacing: 0.03em;
}

.app-bar-link {
  color: inherit;
  text-decoration: none;
}

.nav-btn {
  color: #ffffff;
  font-weight: 700;
  letter-spacing: 0.04em;
}
</style>
