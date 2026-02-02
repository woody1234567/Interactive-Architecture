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
        <li>Left Click + Drag: Rotate</li>
        <li>Right Click + Drag: Pan</li>
        <li>Scroll: Zoom</li>
        <li>Double Click: Focus on object</li>
      </ul>
      <button
        @click="resetCamera"
        class="mt-4 bg-white/20 hover:bg-white/30 text-white px-3 py-1 rounded w-full transition pointer-events-auto"
      >
        Reset Camera
      </button>
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

let camera: THREE.PerspectiveCamera;
let controls: OrbitControls;
let renderer: THREE.WebGLRenderer;
let scene: THREE.Scene;
let raycaster: THREE.Raycaster;
let mouse: THREE.Vector2;
let animationId: number;

const resetCamera = () => {
  if (camera && controls) {
    camera.position.set(10, 10, 10);
    controls.target.set(0, 0, 0);
    controls.update();
  }
};

const onDoubleClick = (event: MouseEvent) => {
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;

  if (camera) {
    raycaster.setFromCamera(mouse, camera);
    if (scene) {
      const intersects = raycaster.intersectObjects(scene.children, true);
      if (intersects.length > 0 && controls) {
        const p = intersects[0].point;
        controls.target.copy(p);
        controls.update();
      }
    }
  }
};

const handleResize = () => {
  if (!container.value || !camera || !renderer) return;
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
};

onMounted(() => {
  if (!container.value) return;

  // Scene setup
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x87ceeb); // Sky blue background
  // scene.fog = new THREE.Fog(0x87ceeb, 10, 100);

  // Camera setup
  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000,
  );
  camera.position.set(10, 10, 10);

  // Renderer setup
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.shadowMap.enabled = true;
  container.value.appendChild(renderer.domElement);

  // Controls
  controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.enablePan = true;
  controls.enableZoom = true;
  controls.dampingFactor = 0.05;
  controls.screenSpacePanning = true;
  // controls.minDistance = 1;
  // controls.maxDistance = 10;
  // controls.maxZoom = 1000;
  // controls.maxPolarAngle = Math.PI / 2; // Don't go below ground

  // Raycaster and Mouse setup
  raycaster = new THREE.Raycaster();
  mouse = new THREE.Vector2();

  // Lights
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(10, 20, 10);
  directionalLight.castShadow = true;
  directionalLight.shadow.camera.top = 20;
  directionalLight.shadow.camera.bottom = -20;
  directionalLight.shadow.camera.left = -20;
  directionalLight.shadow.camera.right = 20;
  scene.add(directionalLight);

  // GLTF Loader
  const loader = new GLTFLoader();
  loader.load(
    "/models/arch_before_with_env/arch_before_with_env.gltf",
    (gltf) => {
      const model = gltf.scene;

      // Center the model
      const box = new THREE.Box3().setFromObject(model);
      const center = box.getCenter(new THREE.Vector3(0, 0, 0));
      model.position.sub(center); // Center at (0,0,0)

      model.traverse((child) => {
        if ((child as THREE.Mesh).isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });

      if (scene) scene.add(model);
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
    },
  );

  window.addEventListener("dblclick", onDoubleClick);
  window.addEventListener("resize", handleResize);

  // Animation loop
  const animate = () => {
    animationId = requestAnimationFrame(animate);

    if (controls) controls.update(); // required if damping enabled
    if (renderer && scene && camera) renderer.render(scene, camera);
  };
  animate();
});

// Cleanup
onUnmounted(() => {
  window.removeEventListener("resize", handleResize);
  window.removeEventListener("dblclick", onDoubleClick);

  if (animationId) cancelAnimationFrame(animationId);

  if (scene) {
    scene.traverse((child) => {
      if ((child as THREE.Mesh).isMesh) {
        const mesh = child as THREE.Mesh;

        if (mesh.geometry) {
          mesh.geometry.dispose();
        }

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

  if (renderer) {
    renderer.dispose();
    renderer.forceContextLoss();
  }

  if (controls) controls.dispose();

  if (
    container.value &&
    renderer &&
    renderer.domElement &&
    container.value.contains(renderer.domElement)
  ) {
    container.value.removeChild(renderer.domElement);
  }
});
</script>
