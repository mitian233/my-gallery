<script setup lang="ts">
defineProps<{
  currentFilter: string;
}>();

const emit = defineEmits<{
  (e: "filter", category: string): void;
}>();

const filters = [
  { id: "all", label: "全部" },
  { id: "landscape", label: "自然风光" },
  { id: "portrait", label: "人像摄影" },
  { id: "architecture", label: "城市建筑" },
];
</script>

<template>
  <header class="pt-32 pb-12 px-6 md:px-12 max-w-7xl">
    <h1
      class="text-5xl md:text-7xl font-serif font-bold mb-6 leading-tight mask-text overflow-hidden"
    >
      <span class="inline-block animate-entry delay-100">捕捉瞬间的</span><br />
      <span class="inline-block animate-entry delay-200">永恒光影</span>
    </h1>

    <div class="flex flex-wrap gap-4 mt-8 animate-entry delay-300">
      <button
        v-for="filter in filters"
        :key="filter.id"
        :class="[
          currentFilter === filter.id
            ? 'bg-white text-black'
            : 'hover:bg-white hover:text-black text-white',
        ]"
        class="filter-btn px-4 py-2 border border-white/20 rounded-full text-sm transition-all duration-300"
        @click="emit('filter', filter.id)"
      >
        {{ filter.label }}
      </button>
    </div>
  </header>
</template>
