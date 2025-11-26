<script setup lang="ts">
import { watch } from "vue";

interface Photo {
  id: number;
  url: string;
  largeUrl: string;
  category: string;
  title: string;
  desc: string;
}

const props = defineProps<{
  isOpen: boolean;
  photo: Photo | null;
}>();

const emit = defineEmits<{
  (e: "close"): void;
}>();

// Lock body scroll when lightbox is open
watch(
  () => props.isOpen,
  (isOpen) => {
    if (typeof document !== "undefined") {
      document.body.style.overflow = isOpen ? "hidden" : "";
    }
  }
);

const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === "Escape" && props.isOpen) {
    emit("close");
  }
};

if (typeof window !== "undefined") {
  window.addEventListener("keydown", handleKeydown);
}
</script>

<template>
  <div id="lightbox" class="fixed inset-0 z-50" :class="{ active: isOpen }">
    <!-- 背景模糊层 -->
    <div
      class="absolute inset-0 bg-black/90 backdrop-blur-md cursor-zoom-out"
      @click="emit('close')"
    ></div>

    <!-- 内容区 -->
    <div
      class="absolute inset-0 flex items-center justify-center p-4 md:p-12 pointer-events-none"
    >
      <div
        v-if="photo"
        class="relative w-full max-w-5xl max-h-full flex flex-col md:flex-row gap-8 pointer-events-auto"
      >
        <!-- 大图 -->
        <div
          class="flex-1 relative overflow-hidden rounded-sm shadow-2xl flex items-center justify-center"
        >
          <img
            id="lightbox-img"
            :src="photo.largeUrl"
            :alt="photo.title"
            class="max-w-full max-h-[85vh] object-contain"
          />
        </div>

        <!-- 信息侧边栏 -->
        <div
          class="lightbox-info-container w-full md:w-80 flex flex-col justify-end text-white pb-4 md:pb-0"
        >
          <span
            class="text-xs font-mono text-gray-400 mb-2 uppercase tracking-widest"
          >
            {{ photo.category }}
          </span>
          <h2 class="text-3xl font-serif mb-4">
            {{ photo.title }}
          </h2>
          <p class="text-sm text-gray-300 leading-relaxed mb-6">
            {{ photo.desc }}
          </p>
          <div
            class="flex items-center gap-4 text-xs text-gray-500 font-mono border-t border-white/10 pt-4 mt-auto"
          >
            <span>ISO 400</span>
            <span>f/2.8</span>
            <span>1/500s</span>
          </div>
        </div>

        <!-- 关闭按钮 -->
        <button
          @click="emit('close')"
          class="absolute -top-12 right-0 md:-right-12 text-white hover:text-gray-300 transition-colors p-2"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="32"
            height="32"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>
