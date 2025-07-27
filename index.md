---
layout: page
title: Home
---

<script type="module">
    import * as THREE from 'https://cdn.jsdelivr.net/npm/three@0.156.1/build/three.module.js';

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x111111);

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 10;

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Conductor
    const conductorGeometry = new THREE.CylinderGeometry(0.2, 0.2, 8, 32);
    const conductorMaterial = new THREE.MeshBasicMaterial({ color: 0xaaaaaa });
    const conductor = new THREE.Mesh(conductorGeometry, conductorMaterial);
    conductor.rotation.z = Math.PI / 2;
    scene.add(conductor);

    // Electron particles
    const particleCount = 100;
    const particles = [];
    const radius = 0.1;
    const spacing = 8 / particleCount;

    for (let i = 0; i < particleCount; i++) {
      const geometry = new THREE.SphereGeometry(radius, 16, 16);
      const material = new THREE.MeshBasicMaterial({ color: 0x00ffff });
      const sphere = new THREE.Mesh(geometry, material);
      sphere.position.x = -4 + i * spacing;
      sphere.position.y = (Math.random() - 0.5) * 0.2;
      particles.push(sphere);
      scene.add(sphere);
    }

    // Animation loop
    function animate() {
      requestAnimationFrame(animate);
      particles.forEach(p => {
        p.position.x += 0.02;
        if (p.position.x > 4) p.position.x = -4;
      });
      renderer.render(scene, camera);
    }

    animate();
  </script>
# Welcome to My Personal Website

Hi there! I'm {{ site.author }}, an Electronics Engineer passionate about technology, programming, and sharing knowledge.

## Latest Blog Posts

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %-d, %Y" }}
{% endfor %}

[View all posts](/archive) | [View my projects](/projects)

## Get In Touch

Feel free to reach out to me through [email]({{ site.email }}) or connect with me on [GitHub](https://github.com/{{ site.github_username }}) and [LinkedIn](https://linkedin.com/in/{{ site.linkedin_username }}).

<style>
  .welcome-section {
    margin-bottom: 2rem;
  }
  
  .featured-posts {
    background: #f8f9fa;
    padding: 1.5rem;
    border-radius: 8px;
    margin: 2rem 0;
  }
  
  .featured-posts h2 {
    margin-top: 0;
    color: #2c3e50;
  }
  
  .cta-buttons {
    display: flex;
    gap: 1rem;
    margin: 2rem 0;
  }
  
  .cta-button {
    display: inline-block;
    padding: 0.8rem 1.5rem;
    background-color: #2a7ae2;
    color: white !important;
    text-decoration: none;
    border-radius: 4px;
    transition: background-color 0.2s;
  }
  
  .cta-button:hover {
    background-color: #1a5cb0;
  }
  
  @media (max-width: 600px) {
    .cta-buttons {
      flex-direction: column;
      gap: 0.5rem;
    }
    
    .cta-button {
      text-align: center;
    }
  }
</style>
