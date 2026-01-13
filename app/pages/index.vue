<template>
  <div class="min-h-screen bg-base-200">
    <!-- Hero Section -->
    <div class="hero min-h-[70vh] bg-base-100">
      <div class="hero-content text-center">
        <div class="max-w-md">
          <h1 class="text-5xl font-bold">Interactive Architecture</h1>
          <p class="py-6">
            Experience architectural modeling like never before. Interact with
            3D models directly in your browser with our powerful web tool.
          </p>
          <NuxtLink to="/arch" class="btn btn-primary">Get Started</NuxtLink>
        </div>
      </div>
    </div>

    <!-- Cards Section -->
    <FeatureCards :items="cards" />
  </div>
</template>

<script setup lang="ts">
import previewArchitecture from "../assets/pictures/arch_preview.png";
import previewCathedral from "../assets/pictures/cathedral_preview.png";
import previewArchwithenv from "../assets/pictures/arch_with_env.png";
const cards = [
  {
    title: "Architecture Demo",
    description:
      "Explore our 3D architectural model with fly controls and interactive elements.",
    image: previewArchitecture,
    link: "/arch",
    buttonText: "View Model",
  },
  {
    title: "Cathedral",
    description: "Orbit control demo with a detailed GLTF model.",
    link: "/cathedral",
    image: previewCathedral,
    buttonText: "View Model",
  },
  {
    title: "Architecture with environment",
    description:
      "Explore our 3D architectural model with fly controls and interactive elements.",
    image: previewArchwithenv,
    link: "/arch_with_env",
    buttonText: "View Model",
  },
];
</script>
