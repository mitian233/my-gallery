<script setup lang="ts">
import GalleryItem from "./GalleryItem.vue";
import type Photo from "@/../shared/types/Photo";

defineProps<{
  photos: Photo[];
  isExiting?: boolean;
  gridKey?: string;
}>();

const emit = defineEmits<{
  (e: "open-lightbox", photo: Photo): void;
}>();
</script>

<template>
  <div class="masonry-grid">
    <GalleryItem
      v-for="photo in photos"
      :key="`${photo.id}-${gridKey || ''}`"
      :photo="photo"
      :is-exiting="isExiting"
      @click="emit('open-lightbox', photo)"
    />
  </div>
</template>
