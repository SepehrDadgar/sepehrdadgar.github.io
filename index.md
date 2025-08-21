---
layout: page
title: Home
---



  <div id="tesseract-container">
    <div id="navbar">
      <a href="#home">Home</a>
      <a href="#projects">Projects</a>
      <a href="#about">About</a>
    </div>
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
      margin: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: #fdfdfd;
      font-family: Arial, sans-serif;
    }

    /* Container for both animation + nav */
    #tesseract-container {
      position: relative;
      width: 400px;
      height: 400px;
      display: flex;
      justify-content: center;
      align-items: center;
      border: 2px solid #ddd;
      border-radius: 10px;
      background: #fff;
      overflow: hidden; /* keeps animation inside */
    }

    canvas {
      display: block;
    }

    /* Navbar inside the container */
    #navbar {
      position: absolute;
      bottom: 10px;
      display: flex;
      gap: 15px;
      z-index: 10;
    }

    #navbar a {
      text-decoration: none;
      color: #333;
      font-weight: bold;
      background: rgba(255,255,255,0.8);
      padding: 5px 10px;
      border-radius: 5px;
      transition: background 0.3s;
    }

    #navbar a:hover {
      background: rgba(200,200,200,0.9);
    }
</style>

<!-- Three.js -->
  <script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
  <script>
    const container = document.getElementById("tesseract-container");
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, 1, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    camera.position.z = 6;

    // Tesseract vertices (4D projected into 3D)
    const vertices4D = [];
    for (let x = -1; x <= 1; x += 2) {
      for (let y = -1; y <= 1; y += 2) {
        for (let z = -1; z <= 1; z += 2) {
          for (let w = -1; w <= 1; w += 2) {
            vertices4D.push([x, y, z, w]);
          }
        }
      }
    }

    function project4Dto3D([x, y, z, w], angle) {
      const cos = Math.cos(angle);
      const sin = Math.sin(angle);
      const xRot = x * cos - w * sin;
      const wRot = x * sin + w * cos;
      const zRot = z * cos - y * sin;
      const yRot = z * sin + y * cos;
      return new THREE.Vector3(xRot, yRot, wRot);
    }

    const material = new THREE.LineBasicMaterial({ color: 0x3333ff });
    const group = new THREE.Group();

    let angle = 0;
    const edges = [];
    for (let i = 0; i < vertices4D.length; i++) {
      for (let j = i + 1; j < vertices4D.length; j++) {
        const diff = vertices4D[i].map((v, k) => Math.abs(v - vertices4D[j][k]));
        if (diff.reduce((a, b) => a + b, 0) === 2) {
          edges.push([i, j]);
        }
      }
    }

    function createTesseract() {
      group.clear();
      const verts3D = vertices4D.map(v => project4Dto3D(v, angle));
      edges.forEach(([i, j]) => {
        const geometry = new THREE.BufferGeometry().setFromPoints([verts3D[i], verts3D[j]]);
        const line = new THREE.Line(geometry, material);
        group.add(line);
      });
    }

    scene.add(group);

    function animate() {
      requestAnimationFrame(animate);
      angle += 0.01;
      createTesseract();
      renderer.render(scene, camera);
    }

    animate();
  </script>