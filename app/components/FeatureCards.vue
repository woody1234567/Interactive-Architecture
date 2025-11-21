<template>
  <div class="container mx-auto p-4">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div
        v-for="(item, index) in items"
        :key="index"
        class="card bg-base-100 shadow-xl"
      >
        <figure v-if="item.image">
          <img :src="item.image" :alt="item.title" />
        </figure>
        <div class="card-body align-center flex">
          <h2 class="card-title">{{ item.title }}</h2>
          <p>{{ item.description }}</p>
          <div class="card-actions justify-center">
            <NuxtLink v-if="item.link" :to="item.link" class="btn btn-primary">
              {{ item.buttonText || "View Demo" }}
            </NuxtLink>
            <button v-else class="btn btn-ghost" disabled>
              {{ item.buttonText || "Coming Soon" }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface CardItem {
  title: string;
  description: string;
  image?: string;
  link?: string;
  buttonText?: string;
}

defineProps<{
  items: CardItem[];
}>();
</script>
