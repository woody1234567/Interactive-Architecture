<template>
  <div class="min-h-screen bg-base-200">
    <!-- Hero Section -->
    <div class="py-20 bg-(--ui-bg-elevated)">
      <UContainer>
        <div class="max-w-2xl mx-auto text-center">
          <h1
            class="text-5xl font-bold tracking-tight text-(--ui-text-highlighted)"
          >
            網頁互動式建築模型
          </h1>
          <p class="py-6 text-lg text-(--ui-text-muted)">
            體驗建築模型互動式模型，透過網頁直接操作建築模型，讓你身歷其境，理解建築的方方面面
          </p>
          <UButton to="/arch" size="xl" icon="i-lucide-arrow-right" trailing>
            開始體驗
          </UButton>
        </div>
      </UContainer>
    </div>

    <!-- Projects Section -->
    <UPageSection title="新營P/S 69kV GIS 新建土建統包工程">
      <FeatureCards :items="cards" />
    </UPageSection>
  </div>
</template>

<script setup lang="ts">
import preview_arch_before from "../assets/pictures/preview_arch_before.png";
import preview_arch_after from "../assets/pictures/preview_arch_after.png";
// import previewCathedral from "../assets/pictures/cathedral_preview.png";
import previewArchwithenv from "../assets/pictures/arch_with_env.png";
const cards = [
  {
    title: "增建前(滑鼠控制)",
    description: "探索建築模型，透過滑鼠控制和互動元素，體驗建築的方方面面",
    image: preview_arch_before,
    link: "/arch_before_mouse",
    buttonText: "體驗模型",
  },
  // {
  //   title: "Cathedral",
  //   description: "Orbit control demo with a detailed GLTF model.",
  //   link: "/cathedral",
  //   image: previewCathedral,
  //   buttonText: "體驗模型",
  // },
  {
    title: "增建前(鍵盤控制)",
    description: "探索建築模型，透過鍵盤控制和互動元素，體驗建築的方方面面",
    image: preview_arch_before,
    link: "/arch_before_keyboard",
    buttonText: "體驗模型",
  },
  {
    title: "增建前(環境模型)",
    description: "觀察建築與環境的關係，更能了解全貌",
    image: previewArchwithenv,
    link: "/arch_before_with_env",
    buttonText: "體驗模型",
  },
  {
    title: "增建後(滑鼠控制)",
    description: "探索建築模型，透過滑鼠控制和互動元素，體驗建築的方方面面",
    image: preview_arch_after,
    link: "/arch_after_mouse",
    buttonText: "體驗模型",
  },
  {
    title: "增建後(鍵盤控制)",
    description: "探索建築模型，透過鍵盤控制和互動元素，體驗建築的方方面面",
    image: preview_arch_after,
    link: "/arch_after_keyboard",
    buttonText: "體驗模型",
  },
];
</script>
