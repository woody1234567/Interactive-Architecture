<template>
  <div ref="container" class="w-full h-screen relative">
    <div
      v-if="loading"
      class="absolute inset-0 flex items-center justify-center bg-black/50 text-white z-10"
    >
      Loading Model... {{ progress.toFixed(0) }}%
    </div>
  </div>
</template>

<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { ref, onMounted, onUnmounted } from "vue";

const container = ref<HTMLDivElement | null>(null);
const loading = ref(true);
const progress = ref(0);

onMounted(() => {
  if (!container.value) return;

  // Scene setup
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0xf0f0f0); // Light gray background

  // Camera setup
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.set(5, 5, 5);

  // Renderer setup
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.shadowMap.enabled = true;
  container.value.appendChild(renderer.domElement);

  // Controls
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true; // Smooth camera movement

  // Lights
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(5, 10, 7.5);
  directionalLight.castShadow = true;
  scene.add(directionalLight);

  // GLTF Loader
  const loader = new GLTFLoader();
  loader.load(
    "/models/architecture.gltf",
    (gltf) => {
      const model = gltf.scene;

      // Center the model
      const box = new THREE.Box3().setFromObject(model);
      const center = box.getCenter(new THREE.Vector3());
      model.position.sub(center); // Center at (0,0,0)

      scene.add(model);
      loading.value = false;
    },
    (xhr) => {
      if (xhr.total > 0) {
        progress.value = (xhr.loaded / xhr.total) * 100;
      }
    },
    (error) => {
      console.error("An error occurred loading the GLTF model:", error);
      loading.value = false;
    }
  );

  // Animation loop
  const animate = () => {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  };
  animate();

  // Handle window resize
  const handleResize = () => {
    if (!container.value) return;
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  };

  window.addEventListener("resize", handleResize);

  // Cleanup
  onUnmounted(() => {
    window.removeEventListener("resize", handleResize);
    renderer.dispose();
    controls.dispose();
    if (container.value) {
      container.value.removeChild(renderer.domElement);
    }
  });
});
</script>
