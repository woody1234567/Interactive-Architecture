<template>
  <div class="navbar bg-base-100 shadow-sm">
    <div class="flex-1">
      <a class="btn btn-ghost text-xl">Architecture</a>
    </div>
    <div class="flex-none">
      <ul class="menu menu-horizontal px-1">
        <li><NuxtLink to="/">Home</NuxtLink></li>
        <li>
          <details>
            <summary>model</summary>
            <ul class="bg-base-100 rounded-t-none p-2">
              <li><NuxtLink to="/architecture">Architecture</NuxtLink></li>
            </ul>
          </details>
        </li>
      </ul>
    </div>
  </div>
  <slot></slot>
</template>
<script lang="ts"></script>
