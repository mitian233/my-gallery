<script setup lang="ts">
import GalleryItem from "./GalleryItem.vue";

interface Photo {
  id: number;
  url: string;
  largeUrl: string;
  category: string;
  title: string;
  desc: string;
}

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
