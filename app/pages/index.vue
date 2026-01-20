<template>
  <div class="min-h-screen bg-base-200">
    <!-- Hero Section -->
    <div class="hero min-h-[70vh] bg-base-100">
      <div class="hero-content text-center">
        <div class="max-w-md">
          <h1 class="text-5xl font-bold">互動式建築模型</h1>
          <p class="py-6">
            體驗建築模型互動式模型，透過網頁直接操作建築模型，讓你身歷其境，理解建築的方方面面
          </p>
          <NuxtLink to="/arch" class="btn btn-primary">開始體驗</NuxtLink>
        </div>
      </div>
    </div>

    <!-- Cards Section -->
    <FeatureCards :items="cards" />
  </div>
</template>

<script setup lang="ts">
import previewArchitecture from "../assets/pictures/arch_preview.png";
// import previewCathedral from "../assets/pictures/cathedral_preview.png";
import previewArchwithenv from "../assets/pictures/arch_with_env.png";
const cards = [
  {
    title: "建築模型(滑鼠控制)",
    description: "探索建築模型，透過滑鼠控制和互動元素，體驗建築的方方面面",
    image: previewArchitecture,
    link: "/arch",
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
    title: "建築模型(鍵盤控制)",
    description: "探索建築模型，透過鍵盤控制和互動元素，體驗建築的方方面面",
    image: previewArchitecture,
    link: "/Control_with_keyboard",
    buttonText: "體驗模型",
  },
  {
    title: "建築模型(環境模)",
    description: "觀察建築與環境的關係，更能了解全貌",
    image: previewArchwithenv,
    link: "/arch_with_env",
    buttonText: "體驗模型",
  },
];
</script>
