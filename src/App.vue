<template>
  <canvas id="canvas" ref="canvas"></canvas>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { Scene, PerspectiveCamera, WebGLRenderer, Mesh, SphereGeometry, PointLight, MeshStandardMaterial } from 'three';
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const canvas = ref<HTMLCanvasElement | null>(null);
const scene = new Scene();

// Create camera
const camera = new PerspectiveCamera(75, window.innerWidth / window.innerHeight, .1, 100);
camera.position.z = 3;
scene.add(camera);

// Create sphere mesh
const geometry = new SphereGeometry(1, 64, 64);
const material = new MeshStandardMaterial({ color: '#00ff83', transparent: true });
const mesh = new Mesh(geometry, material);
mesh.material.opacity =1; // Set desired opacity value here
scene.add(mesh);

// Add point light to illuminate the scene
const light = new PointLight("0xffffff", 1, 100)
light.position.set(0, 5, 5)
scene.add(light)

onMounted(() => {
  const renderer = new WebGLRenderer({
    canvas: canvas.value as HTMLCanvasElement,
    antialias: true,
  })
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.pixelRatio = window.devicePixelRatio;

  const controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true;
  controls.autoRotate = true;
  controls.autoRotateSpeed = 5;

  function animate() {
    requestAnimationFrame(animate);

    controls.update();
    renderer.render(scene, camera)
  }

  animate();
})
</script>

<style scoped></style>