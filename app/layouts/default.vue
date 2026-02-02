<script setup lang="ts">
const navItems = [
  {
    label: "首頁",
    to: "/",
    icon: "i-lucide-house",
  },
];

const dropdownItems = [
  [
    {
      label: "增建前(滑鼠控制)",
      to: "/arch_before_mouse",
      icon: "i-lucide-mouse-pointer-2",
    },
    {
      label: "增建前(鍵盤控制)",
      to: "/arch_before_keyboard",
      icon: "i-lucide-keyboard",
    },
    {
      label: "增建前(環境模型)",
      to: "/arch_before_with_env",
      icon: "i-lucide-mountain",
    },
    {
      label: "增建後(滑鼠控制)",
      to: "/arch_after_mouse",
      icon: "i-lucide-mouse-pointer-2",
    },
    {
      label: "增建後(鍵盤控制)",
      to: "/arch_after_keyboard",
      icon: "i-lucide-keyboard",
    },
  ],
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
          </div>

          <div class="flex items-center gap-4">
            <div class="hidden md:flex items-center gap-2">
              <UNavigationMenu :items="navItems" variant="link" />
              <UDropdownMenu :items="dropdownItems">
                <UButton
                  color="neutral"
                  variant="ghost"
                  label="模型"
                  icon="i-lucide-box"
                  trailing-icon="i-lucide-chevron-down"
                />
              </UDropdownMenu>
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
        </div>
      </UContainer>
    </header>

    <main class="grow">
      <slot />
    </main>

    <AppFooter />
  </div>
</template>
