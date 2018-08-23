<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from "three";
import earthTexture from '../assets/earth_texture.jpg';

export default {
  name: 'Earth',
  data: () => ({ mouseX: 0, mouseY: 0 }),
  mounted() {
    this.init();
    window.addEventListener('mousemove', this.mouseMove, false);
  },
  methods: {
    init() {
      const container = document.getElementById('container');
      const camera = new THREE.PerspectiveCamera( 60, window.innerWidth / window.innerHeight, 1, 2000 );
      const scene = new THREE.Scene();
      const renderer = new THREE.WebGLRenderer();
      const group = new THREE.Group();

      camera.position.z = 500;
      scene.background = new THREE.Color( 0xffffff );
      scene.add(group);

      const texture = new THREE.TextureLoader().load(earthTexture);
      const geometry = new THREE.SphereBufferGeometry( 200, 20, 20 );
      const material = new THREE.MeshBasicMaterial( { map: texture, overdrwa: 0.5 } );
      const mesh = new THREE.Mesh( geometry, material );

      group.add(mesh)
      
      const canvas = document.createElement('canvas');

      canvas.width = 128;
      canvas.height = 128;

      const context = canvas.getContext('2d');
      const gradient = context.createRadialGradient(
          canvas.width / 2,
          canvas.height / 2,
          0,
          canvas.width / 2,
          canvas.height / 2,
          canvas.width / 2
      );

      gradient.addColorStop(0.1, 'rgba(210,210,210,1)');
      gradient.addColorStop(1, 'rgba(255,255,255,1)');
      context.fillStyle = gradient;
      context.fillRect(0, 0, canvas.width, canvas.height);

      renderer.setPixelRatio( window.devicePixelRatio );
      renderer.setSize( window.innerWidth, window.innerHeight );
      container.appendChild(renderer.domElement);

      const animate = () => {
        requestAnimationFrame(animate);

        camera.position.x += ( this.mouseX - camera.position.x ) * 0.05;
				camera.position.y += ( - this.mouseY - camera.position.y ) * 0.05;
				camera.lookAt( scene.position );
				group.rotation.y -= 0.005;
				renderer.render(scene, camera);
      }

      animate();
    },
    mouseMove(e) {
      this.mouseX = ( e.clientX - window.innerWidth / 2 );
      this.mouseY = ( e.clientY - window.innerHeight / 2 );
    }
  },
}
</script>
