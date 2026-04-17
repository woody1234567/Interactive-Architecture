# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Interactive Architecture** is a Nuxt 4 + Vue 3 application for interactive 3D architectural visualization. It showcases building models (before/after construction scenarios) using Three.js with multiple interaction modes:
- **Mouse-based**: OrbitControls for rotating, panning, zooming
- **Keyboard-based**: FlyControls for first-person navigation (WASD + RF + QE for rotation)
- **Environmental context**: Models rendered alongside surrounding environment

Language: TypeScript with Chinese UI text. The app is SSR-disabled (`ssr: false` in nuxt.config.ts).

---

## Common Commands

```bash
# Development
npm run dev              # Start dev server at http://localhost:3000

# Building
npm run build            # Build for production
npm run preview          # Preview production build locally

# Other
npm run generate         # Generate static site
npm install              # Install dependencies (runs postinstall: nuxt prepare)
```

No test suite or linter is currently configured.

---

## Architecture & Structure

### Directory Layout
```
app/
├── pages/          # File-based routing (auto-discovered by Nuxt)
│   ├── index.vue   # Landing page with feature cards and hero section
│   ├── arch_before_mouse.vue          # 3D model with mouse controls (before)
│   ├── arch_before_keyboard.vue       # 3D model with keyboard controls (before)
│   ├── arch_before_with_env.vue       # 3D model with environment context (before)
│   ├── arch_after_mouse.vue           # 3D model with mouse controls (after)
│   ├── arch_after_keyboard.vue        # 3D model with keyboard controls (after)
│   └── cathedral.vue                  # Cathedral reference model (commented out in nav)
├── components/      # Reusable Vue components
│   ├── FeatureCards.vue              # Card grid for showcasing models
│   └── AppFooter.vue                 # Footer (minimal)
├── layouts/
│   └── default.vue  # Default layout wrapper
├── assets/
│   ├── css/tailwind.css              # Tailwind configuration
│   └── pictures/                     # Preview images (PNG)
└── app.vue          # Root app component (UApp + NuxtLayout + NuxtPage)

public/
└── models/          # GLTF model files (arch_before, arch_after, cathedral)
```

### UI Framework
- **@nuxt/ui**: Pre-built Vue components (UButton, UCard, UContainer, UApp, etc.)
- **Tailwind CSS 4.x**: Atomic CSS with @nuxt/ui integration
- **CSS Variables**: Color system defined via `--ui-*` variables (e.g., `bg-(--ui-bg-elevated)`)

### 3D Rendering
- **Three.js 0.181.x**: 3D scene, camera, renderer, lighting
- **OrbitControls**: Mouse-based orbit/pan/zoom (arch_before_mouse, arch_after_mouse)
- **FlyControls**: Keyboard WASD + RF + QE for first-person navigation (arch_before_keyboard, arch_after_keyboard)
- **GLTFLoader**: Load `.gltf` models from `/public/models/`
- **Proper Cleanup**: All scenes dispose geometry, materials, renderer, and remove DOM elements in `onUnmounted()`

---

## Key Patterns & Conventions

### Three.js Scene Setup
All 3D pages follow the same pattern:
1. **Scene + Camera + Renderer** in `onMounted()`
2. **Lighting**: Ambient + Directional light with shadow casting
3. **Ground Plane + Grid**: 100x100 plane with grid helper (visual reference)
4. **Model Loading**: GLTFLoader with progress tracking, center model via Box3, offset y-position if needed
5. **Animation Loop**: `requestAnimationFrame` stored as `let animationId: number`
6. **Resize Handler**: Update camera aspect and renderer size on window resize
7. **Cleanup in onUnmounted()**: Cancel animation frame, dispose all geometries/materials, dispose renderer, remove event listeners, remove DOM element

**Example cleanup pattern** (see arch_before_mouse.vue lines 160–196):
```typescript
onUnmounted(() => {
  window.removeEventListener("resize", handleResize);
  if (animationId) cancelAnimationFrame(animationId);

  // Dispose geometry & materials
  if (scene) {
    scene.traverse((child) => {
      if ((child as THREE.Mesh).isMesh) {
        const mesh = child as THREE.Mesh;
        if (mesh.geometry) mesh.geometry.dispose();
        if (mesh.material) {
          if (Array.isArray(mesh.material)) {
            mesh.material.forEach((m) => m.dispose());
          } else {
            mesh.material.dispose();
          }
        }
      }
    });
  }

  // Dispose renderer & controls
  if (renderer) {
    renderer.dispose();
    renderer.forceContextLoss();
  }
  if (controls) controls.dispose();

  // Remove DOM element
  if (container.value && renderer.domElement && container.value.contains(renderer.domElement)) {
    container.value.removeChild(renderer.domElement);
  }
});
```

### Vue 3 Composition API + Script Setup
All components use `<script setup>` with TypeScript:
- `ref()` for reactive state (container, loading, progress, keyState)
- `onMounted()` for Three.js initialization
- `onUnmounted()` for cleanup
- No Options API or class components

### Model Y-Position Offset
Models are centered via `Box3.getCenter()`, then offset: `model.position.y = -3.4`. This is hand-tuned per model and should not change without re-checking visual alignment.

### Landing Page (index.vue)
- Hero section with title, description, CTA button
- **FeatureCards** grid displaying 5 model demos with images and links
- Cards are responsive: 1 column on mobile, 2 on medium+ screens
- Image imports at top of `<script setup>` (not dynamic paths)

---

## Adding a New 3D Scene Page

1. **Create** `app/pages/your_scene.vue`
2. **Choose interaction type**:
   - Mouse-based: Copy `arch_before_mouse.vue` structure
   - Keyboard-based: Copy `arch_before_keyboard.vue` structure
3. **Update model path**: Change loader.load path from `/models/arch_before/arch_before.gltf` to your model
4. **Tune y-offset**: Adjust `model.position.y` until model sits correctly on ground plane
5. **Update UI text**: Change Chinese control labels in the overlay div (top-left)
6. **Add to index.vue**: Add card object to the `cards` array with image, title, description, link, buttonText
7. **Add preview image**: Place PNG in `app/assets/pictures/` and import it in index.vue

---

## Three.js Model Files

Models are stored in `public/models/`:
- `arch_before/arch_before.gltf` — Architecture before construction
- `arch_after/arch_after.gltf` — Architecture after construction
- `cathedral/scene.gltf` — Reference cathedral model (currently unused)

GLTF files are loaded synchronously but asynchronously via GLTFLoader. Progress is tracked and displayed as percentage.

---

## Nuxt Configuration

Key settings in `nuxt.config.ts`:
- `ssr: false` — Client-side only (required for Three.js)
- `modules: ["@nuxt/ui"]` — Load Nuxt UI
- `css: ["~/assets/css/tailwind.css"]` — Global Tailwind CSS
- `devtools: { enabled: true }` — Nuxt DevTools enabled
- `compatibilityDate: "2025-07-15"` — Nuxt compatibility version

---

## Common Development Tasks

### Modifying 3D Scene Behavior
- **Change camera start position**: Edit `camera.position.set(x, y, z)` in the page component
- **Adjust lighting**: Modify ambient light intensity or directional light position
- **Change model rotation**: Set `model.rotation` before adding to scene
- **Add animations**: Use `clock.getElapsedTime()` or `delta = clock.getDelta()` in animation loop

### Updating UI Components
- **FeatureCards layout**: Adjust grid columns in the `class="grid grid-cols-1 md:grid-cols-2 gap-6"` string
- **Button styling**: @nuxt/ui buttons use `color="primary"` and `variant="*"` props
- **Tailwind utilities**: Use `@nuxt/ui` component props or inline Tailwind classes

### Adding New Pages
- Create `.vue` file in `app/pages/` — Nuxt auto-routes it
- Page name becomes the route (`pages/demo.vue` → `/demo`)
- Use `<NuxtLink to="/path">Link</NuxtLink>` or `<UButton to="/path">` for navigation

### Debugging Three.js
- Check browser console for loader errors: "An error occurred loading the GLTF model"
- Use `console.log(progress)` to verify loading progress tracking
- Inspect scene via Three.js DevTools if available
- Monitor renderer stats (frames per second) via debug info in corner

---

## Notes

- **No API routes**: This is a client-side 3D visualization app. All data (models, images) is static in `public/`.
- **Performance**: Three.js scenes run at full viewport size. Monitor performance on lower-end devices.
- **Accessibility**: 3D scenes have overlay controls but no keyboard-accessible UI—this is a limitation of Three.js interactivity.
- **Responsive Design**: Layout is responsive via Tailwind and @nuxt/ui, but Three.js canvas always fills the viewport (no scaling).
- **Language**: Chinese UI throughout. Change strings in components as needed.
