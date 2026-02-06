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
      <h3 class="font-bold mb-2">控制</h3>
      <ul class="text-sm">
        <li>左鍵 + 拖曳: 旋轉</li>
        <li>右鍵 + 拖曳: 平移</li>
        <li>滾輪: 縮放</li>
      </ul>
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
  scene.background = new THREE.Color(0x87ceeb); // Sky blue background
  // scene.fog = new THREE.Fog(0x87ceeb, 10, 100);

  // Camera setup
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000,
  );
  camera.position.set(0, 30, 40);

  // Renderer setup
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.shadowMap.enabled = true;
  container.value.appendChild(renderer.domElement);

  // Controls
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;
  controls.screenSpacePanning = false;
  controls.minDistance = 1;
  controls.maxDistance = 50;
  controls.maxPolarAngle = Math.PI / 2; // Don't go below ground

  let animationId: number;
  const clock = new THREE.Clock();

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
    "/models/arch_before/arch_before.gltf",
    (gltf) => {
      const model = gltf.scene;

      // Center the model
      const box = new THREE.Box3().setFromObject(model);
      const center = box.getCenter(new THREE.Vector3(0, 0, 0));
      model.position.sub(center); // Center at (0,0,0)

      //   // Lift model to sit on ground
      //   const size = box.getSize(new THREE.Vector3());
      //   // Reset y to 0 after centering, then lift by half height if pivot is center,
      //   // but usually we just want to ensure the bottom is at y=0.
      //   // Let's re-calculate box after centering to be sure.
      //   const box2 = new THREE.Box3().setFromObject(model);
      //   const minY = box2.min.y;
      model.position.y = -3.4;

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
    },
  );

  // Animation loop
  const animate = () => {
    animationId = requestAnimationFrame(animate);

    controls.update(); // required if damping enabled

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
    if (animationId) cancelAnimationFrame(animationId);

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

    if (renderer) {
      renderer.dispose();
      renderer.forceContextLoss();
    }

    if (controls) controls.dispose();

    if (
      container.value &&
      renderer.domElement &&
      container.value.contains(renderer.domElement)
    ) {
      container.value.removeChild(renderer.domElement);
    }
  });
});
</script>
