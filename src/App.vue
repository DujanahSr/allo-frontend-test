<template>
  <v-app>
    <!-- Top Navigation App Bar -->
    <v-app-bar
      flat
      border
      class="px-md-6"
      color="surface"
    >
      <v-container class="d-flex align-center py-0 px-0 max-width-xl">
        <router-link
          to="/"
          class="d-flex align-center text-decoration-none ga-3"
        >
          <v-avatar
            color="primary"
            size="36"
            class="elevation-2"
          >
            <v-icon
              icon="mdi-rocket-launch"
              color="white"
              size="20"
            />
          </v-avatar>
          <div>
            <div class="text-subtitle-1 font-weight-bold text-high-emphasis lh-1">
              SpaceX Explorer
            </div>
            <div class="text-caption text-primary font-weight-medium lh-1">
              Allo Bank Frontend Technical Test
            </div>
          </div>
        </router-link>

        <v-spacer />

        <!-- Candidate Badge -->
        <div class="d-none d-sm-flex align-center ga-2">
          <v-chip
            size="small"
            variant="tonal"
            color="primary"
            class="font-weight-medium"
          >
            <v-icon
              start
              icon="mdi-account-check"
              size="14"
            />
            Abu Dujanah Siregar
          </v-chip>
          <v-btn
            icon="mdi-github"
            variant="text"
            density="comfortable"
            href="https://github.com/DujanahSr/allo-frontend-test"
            target="_blank"
            title="GitHub Repository"
          />
        </div>
      </v-container>
    </v-app-bar>

    <!-- Main View Content -->
    <v-main class="bg-background">
      <router-view />
    </v-main>

    <!-- Footer -->
    <v-footer
      border
      class="bg-surface py-4 text-center d-flex justify-center"
    >
      <div class="text-caption text-medium-emphasis">
        &copy; 2026 <strong>Allo Bank Frontend Assessment</strong> — Built by
        <strong>Abu Dujanah Siregar</strong> with Vue 3, Pinia, Vuetify & TypeScript.
      </div>
    </v-footer>
  </v-app>
</template>

<script lang="ts" setup>
// Main Layout
</script>

<style scoped>
.max-width-xl {
  max-width: 1240px;
}

.lh-1 {
  line-height: 1.2;
}
</style>
