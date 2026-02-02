<template>
  <UContainer class="py-8">
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <UCard
        v-for="(item, index) in items"
        :key="index"
        class="overflow-hidden hover:shadow-lg transition-shadow duration-300"
        :ui="{ body: 'flex flex-col gap-4 items-center text-center' }"
      >
        <template v-if="item.image" #header>
          <img
            :src="item.image"
            :alt="item.title"
            class="w-full h-48 object-cover"
          />
        </template>

        <h2 class="text-xl font-bold">{{ item.title }}</h2>
        <p class="text-muted">{{ item.description }}</p>

        <div class="mt-auto">
          <UButton
            v-if="item.link"
            :to="item.link"
            color="primary"
            class="px-8"
          >
            {{ item.buttonText || "View Demo" }}
          </UButton>
          <UButton v-else color="neutral" variant="ghost" disabled>
            {{ item.buttonText || "Coming Soon" }}
          </UButton>
        </div>
      </UCard>
    </div>
  </UContainer>
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
