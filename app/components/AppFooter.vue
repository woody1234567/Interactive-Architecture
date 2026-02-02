<template>
  <footer class="border-t border-default py-8 bg-default mt-auto">
    <UContainer>
      <div
        class="flex flex-col md:flex-row items-center justify-between gap-4 text-muted text-sm"
      >
        <div class="flex items-center gap-2">
          <UIcon name="i-lucide-building-2" class="w-5 h-5 text-primary" />
          <span class="font-medium">網頁互動式建築模型</span>
        </div>
        <p>© 2026-present StudyWithWoody.</p>
        <div class="flex items-center gap-4">
          <UButton
            icon="i-simple-icons-github"
            color="neutral"
            variant="ghost"
            to="https://github.com"
            target="_blank"
          />
        </div>
      </div>
    </UContainer>
  </footer>
</template>
