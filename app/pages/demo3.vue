<template>
  <div ref="container" class="w-full h-screen relative">
    <div
      v-if="loading"
      class="absolute inset-0 flex items-center justify-center bg-black/50 text-white z-10"
    >
      Loading Model... {{ progress.toFixed(0) }}%
    </div>
    <div
      class="absolute top-4 left-4 bg-black/50 text-white p-4 rounded pointer-events-none"
    >
      <h3 class="font-bold mb-2">Controls</h3>
      <ul class="text-sm">
        <li>W / S: Move Forward / Backward</li>
        <li>A / D: Move Left / Right</li>
        <li>R / F: Move Up / Down</li>
        <li>Q / E: Roll</li>
        <li>Mouse: Look around</li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import * as THREE from "three";
import { FlyControls } from "three/examples/jsm/controls/FlyControls.js";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { ref, onMounted, onUnmounted } from "vue";

const container = ref<HTMLDivElement | null>(null);
const loading = ref(true);
const progress = ref(0);

onMounted(() => {
  if (!container.value) return;

  // Scene setup
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0x87ceeb); // Sky blue background
  scene.fog = new THREE.Fog(0x87ceeb, 10, 100);

  // Camera setup
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.set(0, 10, 30);

  // Renderer setup
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.shadowMap.enabled = true;
  container.value.appendChild(renderer.domElement);

  // Controls
  const controls = new FlyControls(camera, renderer.domElement);
  controls.movementSpeed = 20;
  controls.domElement = renderer.domElement;
  controls.rollSpeed = (5 * Math.PI) / 24;
  controls.autoForward = false;
  controls.dragToLook = true;

  const clock = new THREE.Clock();

  // Lights
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.2);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(10, 20, 10);
  directionalLight.castShadow = true;
  directionalLight.shadow.camera.top = 20;
  directionalLight.shadow.camera.bottom = -20;
  directionalLight.shadow.camera.left = -20;
  directionalLight.shadow.camera.right = 20;
  scene.add(directionalLight);

  // Ground Plane
  const groundGeometry = new THREE.PlaneGeometry(100, 100);
  const groundMaterial = new THREE.MeshStandardMaterial({
    color: 0x808080,
    roughness: 0.8,
    metalness: 0.2,
  });
  const ground = new THREE.Mesh(groundGeometry, groundMaterial);
  ground.rotation.x = -Math.PI / 2; // Rotate to be horizontal
  ground.receiveShadow = true;
  scene.add(ground);

  // Grid Helper
  const gridHelper = new THREE.GridHelper(100, 100);
  scene.add(gridHelper);

  // GLTF Loader
  const loader = new GLTFLoader();
  loader.load(
    "/models/demo3.gltf",
    (gltf) => {
      const model = gltf.scene;

      // Center the model
      const box = new THREE.Box3().setFromObject(model);
      const center = box.getCenter(new THREE.Vector3(0, 0, 0));
      model.position.sub(center); // Center at (0,0,0)

      // Lift model to sit on ground if needed (assuming model bottom is at y=0 relative to its center)
      // Adjusting y position so the bottom of the model is at y=0
      //   const size = box.getSize(new THREE.Vector3());
      //   model.position.y += size.y / 2;
      model.position.y = -3.5;

      model.traverse((child) => {
        if ((child as THREE.Mesh).isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });

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

    const delta = clock.getDelta();
    controls.update(delta);

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
