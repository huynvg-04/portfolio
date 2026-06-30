<script setup>
import { ref, onMounted, onUnmounted, shallowRef } from 'vue';
import * as THREE from 'three';

const container = ref(null);
const scene = shallowRef(null);
const camera = shallowRef(null);
const renderer = shallowRef(null);
const material = shallowRef(null);

let animationId = null;
const mouse = new THREE.Vector2(-9999, -9999);
const raycaster = new THREE.Raycaster();
const mouse3D = new THREE.Vector3(-9999, -9999, -9999);

// Hidden plane for raycasting
const plane = new THREE.Mesh(
  new THREE.PlaneGeometry(2000, 2000),
  new THREE.MeshBasicMaterial({ visible: false })
);

const handleMouseMove = (event) => {
  // Normalize mouse coordinates from -1 to 1
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
};

const handleResize = () => {
  if (!camera.value || !renderer.value) return;
  camera.value.aspect = window.innerWidth / window.innerHeight;
  camera.value.updateProjectionMatrix();
  renderer.value.setSize(window.innerWidth, window.innerHeight);
};

onMounted(() => {
  // Setup Scene
  scene.value = new THREE.Scene();
  scene.value.add(plane);

  // Setup Camera
  camera.value = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.value.position.z = 15;

  // Setup Renderer
  renderer.value = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.value.setSize(window.innerWidth, window.innerHeight);
  renderer.value.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  container.value.appendChild(renderer.value.domElement);

  // Particles
  const particleCount = 2000;
  const geometry = new THREE.BufferGeometry();
  const positions = new Float32Array(particleCount * 3);
  const sizes = new Float32Array(particleCount);

  for (let i = 0; i < particleCount; i++) {
    // Distribute particles inside a large volume
    positions[i * 3] = (Math.random() - 0.5) * 50; // x
    positions[i * 3 + 1] = (Math.random() - 0.5) * 50; // y
    positions[i * 3 + 2] = (Math.random() - 0.5) * 20; // z
    
    sizes[i] = Math.random() * 2.0 + 0.5;
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  geometry.setAttribute('aInitialPosition', new THREE.BufferAttribute(positions, 3));
  geometry.setAttribute('aSize', new THREE.BufferAttribute(sizes, 1));

  const vertexShader = `
    uniform float uTime;
    uniform vec3 uMouse;
    attribute vec3 aInitialPosition;
    attribute float aSize;

    void main() {
      vec3 pos = aInitialPosition;
      
      // Gentle floating animation
      pos.y += sin(uTime * 0.5 + pos.x * 0.5) * 0.5;
      pos.z += cos(uTime * 0.3 + pos.y * 0.5) * 0.5;
      
      // Repel logic
      float dist = distance(pos, uMouse);
      float repelRadius = 6.0;
      if (dist < repelRadius) {
        vec3 dir = normalize(pos - uMouse);
        float force = (repelRadius - dist) / repelRadius;
        force = smoothstep(0.0, 1.0, force);
        pos += dir * force * 3.5;
      }
      
      vec4 mvPosition = modelViewMatrix * vec4(pos, 1.0);
      gl_PointSize = aSize * (30.0 / -mvPosition.z);
      gl_Position = projectionMatrix * mvPosition;
    }
  `;

  const fragmentShader = `
    uniform vec3 uColor;

    void main() {
      float dist = distance(gl_PointCoord, vec2(0.5));
      if (dist > 0.5) discard;
      float alpha = smoothstep(0.5, 0.1, dist);
      gl_FragColor = vec4(uColor, alpha * 0.6);
    }
  `;

  material.value = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    transparent: true,
    depthWrite: false,
    blending: THREE.AdditiveBlending,
    uniforms: {
      uTime: { value: 0 },
      uMouse: { value: new THREE.Vector3(-9999, -9999, -9999) },
      uColor: { value: new THREE.Color(0xd6adff) } // Matches the neon purple/pink accent
    }
  });

  const particles = new THREE.Points(geometry, material.value);
  scene.value.add(particles);

  // Animation Loop
  const clock = new THREE.Clock();
  
  const animate = () => {
    const elapsedTime = clock.getElapsedTime();
    
    // Update Raycaster for mouse
    raycaster.setFromCamera(mouse, camera.value);
    const intersects = raycaster.intersectObject(plane);
    if (intersects.length > 0) {
      // Smoothly interpolate mouse3D for smoother repel
      mouse3D.lerp(intersects[0].point, 0.1);
    } else {
      mouse3D.set(-9999, -9999, -9999);
    }
    
    if (material.value) {
      material.value.uniforms.uTime.value = elapsedTime;
      material.value.uniforms.uMouse.value.copy(mouse3D);
    }

    renderer.value.render(scene.value, camera.value);
    animationId = requestAnimationFrame(animate);
  };

  animate();

  window.addEventListener('resize', handleResize);
  window.addEventListener('mousemove', handleMouseMove);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
  window.removeEventListener('mousemove', handleMouseMove);
  if (animationId) cancelAnimationFrame(animationId);
  
  if (renderer.value) {
    renderer.value.dispose();
  }
  if (material.value) {
    material.value.dispose();
  }
});
</script>

<template>
  <div ref="container" class="particle-background"></div>
</template>

<style scoped>
.particle-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none; /* Allows clicking links underneath */
}
</style>
