<template>
  <v-container class="py-6 py-md-10 max-width-xl">
    <!-- Header Hero Section -->
    <v-row class="mb-6 align-center">
      <v-col
        cols="12"
        md="8"
      >
        <div class="d-flex align-center ga-3 mb-2">
          <v-chip
            color="primary"
            variant="flat"
            size="small"
            class="font-weight-bold text-uppercase"
          >
            Allo Bank Assessment
          </v-chip>
          <span class="text-caption text-medium-emphasis">SpaceX Catalog API v2.2.0</span>
        </div>
        <h1 class="text-h4 text-md-h3 font-weight-bold text-high-emphasis tracking-tight">
          SpaceX Rocket Fleet
        </h1>
        <p class="text-body-1 text-medium-emphasis mt-2 mb-0">
          Eksplorasi armada roket SpaceX dari peluncur Falcon 1 hingga Starship generasi berikutnya.
        </p>
      </v-col>

      <v-col
        cols="12"
        md="4"
        class="text-md-right"
      >
        <AddRocketDialog @created="onRocketAdded" />
      </v-col>
    </v-row>

    <!-- Search & Filter Controls -->
    <v-card
      class="pa-4 mb-8 rounded-xl elevation-1 border"
      color="surface"
    >
      <v-row
        align="center"
        dense
      >
        <v-col
          cols="12"
          sm="8"
          md="9"
        >
          <v-text-field
            v-model="searchQuery"
            placeholder="Cari roket berdasarkan nama atau deskripsi..."
            prepend-inner-icon="mdi-magnify"
            clearable
            hide-details
            density="comfortable"
            variant="solo-filled"
            flat
            rounded="lg"
            class="search-input"
          />
        </v-col>

        <v-col
          cols="12"
          sm="4"
          md="3"
          class="text-sm-right mt-2 mt-sm-0"
        >
          <v-chip
            color="primary"
            variant="tonal"
            class="font-weight-bold"
          >
            {{ rocketStore.filteredRockets.length }} dari {{ rocketStore.totalCount }} Roket
          </v-chip>
        </v-col>
      </v-row>
    </v-card>

    <!-- 1. LOADING STATE -->
    <v-row v-if="rocketStore.isLoading">
      <v-col
        v-for="n in 6"
        :key="n"
        cols="12"
        sm="6"
        md="4"
      >
        <RocketSkeleton />
      </v-col>
    </v-row>

    <!-- 2. ERROR / FAIL STATE (WITH RETRY BUTTON) -->
    <ErrorState
      v-else-if="rocketStore.error"
      title="Gagal Memuat Katalog Roket"
      :message="rocketStore.error"
      :loading="rocketStore.isLoading"
      @retry="rocketStore.fetchRockets(true)"
    />

    <!-- 3. SUCCESS STATE: EMPTY SEARCH RESULT -->
    <v-card
      v-else-if="rocketStore.filteredRockets.length === 0"
      class="pa-12 text-center rounded-xl elevation-1 border my-6"
      color="surface"
    >
      <v-avatar
        color="primary-lighten-5"
        size="64"
        class="mb-4 text-primary"
      >
        <v-icon
          icon="mdi-magnify-close"
          size="36"
        />
      </v-avatar>
      <h3 class="text-h6 font-weight-bold mb-2">
        Tidak Ada Roket yang Cocok
      </h3>
      <p class="text-body-2 text-medium-emphasis mb-4">
        Tidak ditemukan roket dengan kata kunci "<strong>{{ searchQuery }}</strong>". Coba kata kunci lain.
      </p>
      <v-btn
        color="primary"
        variant="tonal"
        prepend-icon="mdi-close-circle-outline"
        @click="searchQuery = ''"
      >
        Hapus Filter Pencarian
      </v-btn>
    </v-card>

    <!-- 4. SUCCESS STATE: ROCKETS GRID -->
    <v-row v-else>
      <v-col
        v-for="rocket in rocketStore.filteredRockets"
        :key="rocket.id"
        cols="12"
        sm="6"
        md="4"
        class="d-flex"
      >
        <RocketCard
          :rocket="rocket"
          @select="navigateToDetail"
        />
      </v-col>
    </v-row>

    <!-- Snackbar Notification for Added Rocket -->
    <v-snackbar
      v-model="showSuccessSnackbar"
      color="success"
      location="bottom right"
      timeout="3500"
      rounded="pill"
    >
      <v-icon
        start
        icon="mdi-check-circle"
      />
      Roket baru berhasil ditambahkan ke katalog!
    </v-snackbar>
  </v-container>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { useRocketStore } from '@/stores/rocket'
import RocketCard from '@/components/RocketCard.vue'
import RocketSkeleton from '@/components/RocketSkeleton.vue'
import ErrorState from '@/components/ErrorState.vue'
import AddRocketDialog from '@/components/AddRocketDialog.vue'

const router = useRouter()
const rocketStore = useRocketStore()
const showSuccessSnackbar = ref(false)

const searchQuery = computed({
  get: () => rocketStore.searchQuery,
  set: (val: string) => rocketStore.setSearchQuery(val || ''),
})

onMounted(() => {
  rocketStore.initCustomRockets()
  rocketStore.fetchRockets()
})

function navigateToDetail(id: string | number) {
  router.push(`/rockets/${id}`)
}

function onRocketAdded() {
  showSuccessSnackbar.value = true
}
</script>

<style scoped>
.max-width-xl {
  max-width: 1240px;
}

.tracking-tight {
  letter-spacing: -0.02em;
}
</style>
