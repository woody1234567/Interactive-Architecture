<template>
  <div class="navbar bg-base-100 shadow-sm">
    <div class="flex-1">
      <NuxtLink to="/" class="btn btn-ghost text-xl">Architecture</NuxtLink>
    </div>
    <div class="flex-none">
      <ul class="menu menu-horizontal px-1">
        <li><NuxtLink to="/">Home</NuxtLink></li>
        <li>
          <details>
            <summary>model</summary>
            <ul class="bg-base-100 rounded-t-none p-2 z-30">
              <li><NuxtLink to="/demo1">Demo1</NuxtLink></li>
              <li><NuxtLink to="/demo2">Demo2</NuxtLink></li>
            </ul>
          </details>
        </li>
      </ul>
    </div>
  </div>
  <slot></slot>
</template>
<script lang="ts"></script>
