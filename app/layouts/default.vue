<template>
  <div class="flex flex-col min-h-screen">
    <div class="navbar bg-base-100 shadow-sm">
      <div class="flex-1">
        <NuxtLink to="/" class="btn btn-ghost text-xl"
          >網頁互動式建築模型</NuxtLink
        >
      </div>
      <div class="flex-none">
        <ul class="menu menu-horizontal px-1">
          <li><NuxtLink to="/">首頁</NuxtLink></li>
          <li>
            <details>
              <summary>模型</summary>
              <ul class="bg-base-100 rounded-t-none p-2 z-30">
                <li><NuxtLink to="/arch">建築模型(滑鼠控制)</NuxtLink></li>
                <li>
                  <NuxtLink to="/Control_with_keyboard"
                    >建築模型(鍵盤控制)</NuxtLink
                  >
                </li>

                <!-- <li><NuxtLink to="/cathedral">cathedral</NuxtLink></li> -->
                <li><NuxtLink to="/arch_with_env">環境模</NuxtLink></li>
              </ul>
            </details>
          </li>
        </ul>
      </div>
    </div>
    <div class="grow">
      <slot></slot>
    </div>
    <AppFooter />
  </div>
</template>
<script lang="ts"></script>
