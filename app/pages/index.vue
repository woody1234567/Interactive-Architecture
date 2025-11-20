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
          <NuxtLink to="/architecture" class="btn btn-primary"
            >Get Started</NuxtLink
          >
        </div>
      </div>
    </div>

    <!-- Cards Section -->
    <div class="container mx-auto p-4">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <!-- Architecture Card -->
        <div class="card bg-base-100 shadow-xl">
          <figure>
            <img
              src="../assets/pictures/preview_architecture.png"
              alt="Architecture"
            />
          </figure>
          <div class="card-body">
            <h2 class="card-title">Architecture Demo</h2>
            <p>
              Explore our 3D architectural model with fly controls and
              interactive elements.
            </p>
            <div class="card-actions justify-end">
              <NuxtLink to="/architecture" class="btn btn-primary"
                >View Model</NuxtLink
              >
            </div>
          </div>
        </div>

        <!-- Placeholder Card 2 -->
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h2 class="card-title">Future Project</h2>
            <p>More interactive models coming soon.</p>
            <div class="card-actions justify-end">
              <button class="btn btn-ghost" disabled>Coming Soon</button>
            </div>
          </div>
        </div>

        <!-- Placeholder Card 3 -->
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h2 class="card-title">Documentation</h2>
            <p>Learn how to use the tools and controls.</p>
            <div class="card-actions justify-end">
              <button class="btn btn-ghost">Read Docs</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// No script needed for this layout
</script>
