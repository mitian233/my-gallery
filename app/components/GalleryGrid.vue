<script setup lang="ts">
import GalleryItem from "./GalleryItem.vue";
import type Photo from "@/../shared/types/Photo";

defineProps<{
  photos: Photo[];
}>();

const emit = defineEmits<{
  (e: "open-lightbox", photo: Photo): void;
}>();
</script>

<template>
  <div class="masonry-grid">
    <GalleryItem
      v-for="photo in photos"
      :key="photo.id"
      :photo="photo"
      @click="emit('open-lightbox', photo)"
    />
  </div>
</template>
