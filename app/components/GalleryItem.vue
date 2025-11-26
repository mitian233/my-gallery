<script setup lang="ts">
import { onMounted, ref, onUnmounted } from "vue";

interface Photo {
  id: number;
  url: string;
  largeUrl: string;
  category: string;
  title: string;
  desc: string;
}

const props = defineProps<{
  photo: Photo;
}>();

const emit = defineEmits<{
  (e: "click", photo: Photo): void;
}>();

const itemRef = ref<HTMLElement | null>(null);
let observer: IntersectionObserver | null = null;

onMounted(() => {
  if (!itemRef.value) return;

  const observerOptions = {
    root: null,
    rootMargin: "0px",
    threshold: 0.1,
  };

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add("in-view");
        observer?.unobserve(entry.target);
      }
    });
  }, observerOptions);

  observer.observe(itemRef.value);
});

onUnmounted(() => {
  if (observer) {
    observer.disconnect();
  }
});
</script>

<template>
  <div
    ref="itemRef"
    class="masonry-item relative group cursor-pointer overflow-hidden rounded-sm"
    @click="emit('click', photo)"
  >
    <img
      :src="photo.url"
      :alt="photo.title"
      class="w-full h-auto block transform transition-transform duration-700 group-hover:scale-110 will-change-transform"
      loading="lazy"
    />
    <div
      class="absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex flex-col justify-end p-6 overlay-gradient"
    >
      <span
        class="text-xs text-gray-300 font-mono uppercase tracking-wider mb-1 translate-y-4 group-hover:translate-y-0 transition-transform duration-300"
      >
        {{ photo.category }}
      </span>
      <h3
        class="text-white font-serif text-xl translate-y-4 group-hover:translate-y-0 transition-transform duration-300 delay-75"
      >
        {{ photo.title }}
      </h3>
    </div>
  </div>
</template>
