<script setup lang="ts">
import { ref, computed } from "vue";
import TheNavbar from "~/components/TheNavbar.vue";
import TheFooter from "~/components/TheFooter.vue";
import GalleryHeader from "~/components/GalleryHeader.vue";
import GalleryGrid from "~/components/GalleryGrid.vue";
import GalleryLightbox from "~/components/GalleryLightbox.vue";
import type Photo from "~~/shared/types/Photo";

useHead({
  title: "Lumina | 极简视觉流",
  meta: [{ name: "description", content: "捕捉瞬间的永恒光影" }],
});

// Mock Data
const photoData = [
  {
    id: 1,
    url: "https://images.unsplash.com/photo-1493246507139-91e8fad9978e?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1493246507139-91e8fad9978e?w=1600&q=90",
    category: "landscape",
    title: "高山迷雾",
    desc: "清晨的阿尔卑斯山脉，雾气缭绕在针叶林之间。",
  },
  {
    id: 2,
    url: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=1600&q=90",
    category: "portrait",
    title: "街头回眸",
    desc: "纽约街头偶然捕捉到的瞬间，光影完美地勾勒出轮廓。",
  },
  {
    id: 3,
    url: "https://images.unsplash.com/photo-1479839672679-a46483c0e7c8?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1479839672679-a46483c0e7c8?w=1600&q=90",
    category: "architecture",
    title: "几何秩序",
    desc: "现代主义建筑的线条美学，黑与白的极致对比。",
  },
  {
    id: 4,
    url: "https://images.unsplash.com/photo-1504674900247-0877df9cc836?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1504674900247-0877df9cc836?w=1600&q=90",
    category: "landscape",
    title: "风暴前夕",
    desc: "大西洋海岸线上的乌云密布，海浪拍打着礁石。",
  },
  {
    id: 5,
    url: "https://images.unsplash.com/photo-1517841905240-472988babdf9?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1517841905240-472988babdf9?w=1600&q=90",
    category: "portrait",
    title: "自然神态",
    desc: "无需摆拍的真实流露，使用了 85mm 镜头拍摄。",
  },
  {
    id: 6,
    url: "https://images.unsplash.com/photo-1518005052304-a37d996b0756?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1518005052304-a37d996b0756?w=1600&q=90",
    category: "architecture",
    title: "旋转阶梯",
    desc: "梵蒂冈博物馆的螺旋楼梯，经典的构图视角。",
  },
  {
    id: 7,
    url: "https://images.unsplash.com/photo-1500964757637-c85e8a162699?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1500964757637-c85e8a162699?w=1600&q=90",
    category: "landscape",
    title: "秋日私语",
    desc: "金色的阳光透过树叶洒在地面上，温暖的色调。",
  },
  {
    id: 8,
    url: "https://images.unsplash.com/photo-1481349518771-20055b2a7b24?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1481349518771-20055b2a7b24?w=1600&q=90",
    category: "architecture",
    title: "玻璃幕墙",
    desc: "反射着蓝天的摩天大楼细节。",
  },
  {
    id: 9,
    url: "https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?w=1600&q=90",
    category: "portrait",
    title: "光影游戏",
    desc: "百叶窗投下的阴影创造出的独特纹理。",
  },
  {
    id: 10,
    url: "https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=1600&q=90",
    category: "landscape",
    title: "孤寂之路",
    desc: "通向远方的公路，象征着探索与未知。",
  },
  {
    id: 11,
    url: "https://images.unsplash.com/photo-1497366216548-37526070297c?w=800&q=80",
    largeUrl:
      "https://images.unsplash.com/photo-1497366216548-37526070297c?w=1600&q=90",
    category: "architecture",
    title: "极简空间",
    desc: "纯白色的室内设计，强调光线的流动。",
  },
];

// State
const currentFilter = ref("all");
const isLightboxOpen = ref(false);
const lightboxPhoto = ref<Photo | null>(null);
const isGridExiting = ref(false);

// Computed
const filteredPhotos = computed(() => {
  if (currentFilter.value === "all") {
    return photoData;
  }
  return photoData.filter((p) => p.category === currentFilter.value);
});

// Methods
const setFilter = async (filter: string) => {
  if (currentFilter.value === filter) return;

  // Start exit animation
  isGridExiting.value = true;

  // Wait for exit animation to complete (0.8s matches CSS transition)
  await new Promise((resolve) => setTimeout(resolve, 800));

  currentFilter.value = filter;
  
  // Reset exit state
  isGridExiting.value = false;
};

const openLightbox = (photo: Photo) => {
  lightboxPhoto.value = photo;
  isLightboxOpen.value = true;
};

const closeLightbox = () => {
  isLightboxOpen.value = false;
  // Wait for transition to finish before clearing photo (optional, but good for UX)
  setTimeout(() => {
    if (!isLightboxOpen.value) {
      lightboxPhoto.value = null;
    }
  }, 300);
};
</script>

<template>
  <div
    class="antialiased selection:bg-white selection:text-black min-h-screen flex flex-col"
  >
    <TheNavbar />

    <GalleryHeader :current-filter="currentFilter" @filter="setFilter" />

    <main class="px-6 md:px-12 pb-24 max-w-[1600px] mx-auto w-full grow">
      <GalleryGrid
        :photos="filteredPhotos"
        :is-exiting="isGridExiting"
        :grid-key="currentFilter"
        @open-lightbox="openLightbox"
      />
    </main>

    <TheFooter />

    <GalleryLightbox
      :is-open="isLightboxOpen"
      :photo="lightboxPhoto"
      @close="closeLightbox"
    />
  </div>
</template>
