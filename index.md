---
layout: page
title: Home
---

# Welcome to My Personal Website
  <div class="model-box">
    <canvas id="modelCanvas" width="320" height="320"></canvas>
  </div>

Hi there! I'm {{ site.author }}, an Electronics Engineer passionate about technology, programming, and sharing knowledge.

مرا نه دولت وصل و نه احتمال فراق

نه پای رفتن از این ناحیت نه جای مُقام

___
... and then there is necessity and the needs of the whole world, of which you are a part of. whatever the nature of the whole does, and whatever serves to maintain it, it is good for every part of nature. the world is maintained by change--in elements and in the things they compose. that should be enough for you; treat it as an axiom. ... -Meditations Book 2, 3
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

    body {
      background: #fdfdfd;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    /* container with visible overflow */
    .model-box {
      width: 320px;
      height: 320px;
      position: relative;
      overflow: visible;
    }

    canvas {
      width: 100%;
      height: 100%;
      display: block;
    }
</style>

  <script type="module">
    import * as THREE from "https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.module.js";
    import { OBJLoader } from "https://cdn.jsdelivr.net/npm/three@0.158.0/examples/jsm/loaders/OBJLoader.js";

    const canvas = document.getElementById("modelCanvas");

    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, 1, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true });
    renderer.setClearColor(0xfdfdfd, 1);

    // Lighting
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(3, 3, 5);
    scene.add(light);
    scene.add(new THREE.AmbientLight(0xffffff, 0.4));

    // Load OBJ model
    const loader = new OBJLoader();
    loader.load("model.obj", (object) => {
      object.traverse((child) => {
        if (child.isMesh) {
          child.material = new THREE.MeshStandardMaterial({ color: 0x444444 });
        }
      });

      object.scale.set(1.2, 1.2, 1.2); // slightly oversized to peek out
      object.rotation.x = 0.5;
      scene.add(object);

      animate(object);
    });

    camera.position.z = 4;

    function animate(model) {
      requestAnimationFrame(() => animate(model));
      model.rotation.y += 0.01;
      renderer.render(scene, camera);
    }
  </script>