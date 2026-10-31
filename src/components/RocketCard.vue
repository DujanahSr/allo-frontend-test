<template>
  <v-card
    class="rocket-card h-100 d-flex flex-column rounded-xl transition-swing elevation-2 hover-elevation-8"
    hover
    @click="$emit('select', rocket.id)"
  >
    <div
      class="position-relative overflow-hidden"
      style="height: 220px; background-color: #111b27;"
    >
      <v-img
        :src="imageUrl"
        :alt="rocket.full_name"
        height="220"
        cover
        class="rocket-image"
        @error="onImageError"
      >
        <template #placeholder>
          <div class="d-flex align-center justify-center fill-height bg-grey-darken-3">
            <v-progress-circular
              indeterminate
              color="primary"
            />
          </div>
        </template>
      </v-img>

      <!-- Badge Country & Custom Rocket -->
      <div
        class="position-absolute d-flex ga-2"
        style="top: 12px; left: 12px;"
      >
        <v-chip
          v-if="rocket.manufacturer?.country_code"
          size="small"
          color="surface"
          variant="flat"
          class="font-weight-bold text-uppercase elevation-2"
        >
          <v-icon
            start
            icon="mdi-flag"
            size="14"
          />
          {{ rocket.manufacturer.country_code }}
        </v-chip>

        <v-chip
          v-if="rocket.isCustom"
          size="small"
          color="amber-accent-4"
          variant="flat"
          class="font-weight-bold elevation-2"
        >
          <v-icon
            start
            icon="mdi-star"
            size="14"
          />
          Custom
        </v-chip>
      </div>
    </div>

    <v-card-item class="pb-1 pt-3">
      <v-card-title
        class="text-h6 font-weight-bold text-truncate"
        :title="rocket.full_name"
      >
        {{ rocket.full_name }}
      </v-card-title>
    </v-card-item>

    <v-card-text class="flex-grow-1 text-body-2 text-medium-emphasis pt-1 pb-3">
      <p class="description-text mb-0">
        {{ displayDescription }}
      </p>
    </v-card-text>

    <v-divider />

    <v-card-actions class="px-4 py-3 bg-surface-light d-flex justify-space-between align-center">
      <span class="text-caption font-weight-medium text-primary d-flex align-center">
        Lihat Spesifikasi
        <v-icon
          end
          icon="mdi-arrow-right"
          size="16"
        />
      </span>
      <v-btn
        variant="text"
        color="primary"
        size="small"
        icon="mdi-chevron-right"
        density="comfortable"
      />
    </v-card-actions>
  </v-card>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import type { Rocket } from '@/types/rocket'
import { FALLBACK_ROCKET_IMAGE } from '@/utils/formatters'

const props = defineProps<{
  rocket: Rocket
}>()

defineEmits<{
  (e: 'select', id: string | number): void
}>()

const imageFailed = ref(false)

const imageUrl = computed(() => {
  if (imageFailed.value || !props.rocket.image_url) {
    return FALLBACK_ROCKET_IMAGE
  }
  return props.rocket.image_url
})

const displayDescription = computed(() => {
  if (!props.rocket.description || props.rocket.description.trim() === '') {
    return 'Deskripsi roket belum tersedia dalam dokumentasi sistem.'
  }
  return props.rocket.description
})

function onImageError() {
  imageFailed.value = true
}
</script>

<style scoped>
.rocket-card {
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  cursor: pointer;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

.rocket-card:hover {
  transform: translateY(-4px);
}

.rocket-image {
  transition: transform 0.35s ease;
}

.rocket-card:hover .rocket-image {
  transform: scale(1.05);
}

.description-text {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  line-height: 1.5;
}
</style>
