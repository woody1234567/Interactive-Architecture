<script setup lang="ts">
const items = [
  {
    label: "首頁",
    to: "/",
    icon: "i-lucide-house",
  },
  {
    label: "模型",
    icon: "i-lucide-box",
    children: [
      {
        label: "建築模型(滑鼠控制)",
        to: "/arch",
        icon: "i-lucide-mouse-pointer-2",
      },
      {
        label: "建築模型(鍵盤控制)",
        to: "/Control_with_keyboard",
        icon: "i-lucide-keyboard",
      },
      {
        label: "環境模型",
        to: "/arch_with_env",
        icon: "i-lucide-mountain",
      },
    ],
  },
];
</script>

<template>
  <div class="flex flex-col min-h-screen">
    <header class="border-b border-default sticky top-0 bg-default z-50">
      <UContainer>
        <div class="flex items-center justify-between h-16">
          <div class="flex items-center gap-8">
            <NuxtLink to="/" class="text-xl font-bold flex items-center gap-2">
              <UIcon name="i-lucide-building-2" class="w-6 h-6" />
              <span>網頁互動式建築模型</span>
            </NuxtLink>
            <UNavigationMenu
              :items="items"
              variant="pill"
              class="hidden md:flex"
            />
          </div>
          <div class="flex items-center gap-2">
            <UColorModeButton />
            <UButton
              color="neutral"
              variant="ghost"
              icon="i-lucide-menu"
              class="md:hidden"
            />
          </div>
        </div>
      </UContainer>
    </header>

    <main class="grow">
      <slot />
    </main>

    <AppFooter />
  </div>
</template>
