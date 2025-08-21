---
layout: page
title: Sepehr's Blog 🙄 ☜(ﾟヮﾟ☜)
---



  <div id="container"></div>


Hi there! I'm {{ site.author }}, an Electronics Engineer.

مرا نه دولت وصل و نه احتمال فراق

نه پای رفتن از این ناحیت نه جای مُقام

___
... and then there is necessity and the needs of the whole world, of which you are a part of. whatever the nature of the whole does, and whatever serves to maintain it, it is good for every part of nature. the world is maintained by change--in elements and in the things they compose. that should be enough for you; treat it as an axiom. ... -Meditations Book 2, 3
## Latest Blog Posts

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %-d, %Y" }}
{% endfor %}


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
      background: #fdfdfd;
      font-family: sans-serif;
    }


    nav {
      margin: 20px;
    }

    nav a {
      margin: 0 10px;
      text-decoration: none;
      color: #333;
      font-weight: bold;
    }

    #container {
      width: 300px;
      height: 300px;
      position: relative;
      overflow: hidden; /* keeps it from going over other elements */
      margin: 0 auto;
    }

    canvas {
      display: block;
    }

    .post-title {
      margin-bottom: 0.5rem;
      font-size: 1.7rem;
    }
</style>

<script src="https://cdn.jsdelivr.net/npm/three@0.156.1/build/three.min.js"></script>
  <script>
    const container = document.getElementById("container");

    // scene
    const scene = new THREE.Scene();

    // camera
    const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
    camera.position.z = 5;

    // renderer
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    // function to create a cube wireframe at scale
    function createCube(scale) {
      const geometry = new THREE.BoxGeometry(scale, scale, scale);
      const edges = new THREE.EdgesGeometry(geometry);
      const material = new THREE.LineBasicMaterial({ color: 0x3333ff });
      return new THREE.LineSegments(edges, material);
    }

    // tesseract = inner cube + outer cube + connecting edges
    const innerCube = createCube(1);
    const outerCube = createCube(2);

    scene.add(innerCube);
    scene.add(outerCube);

    // connect edges between inner and outer cubes
    const points = [];
    const innerVertices = innerCube.geometry.attributes.position.array;
    const outerVertices = outerCube.geometry.attributes.position.array;

    for (let i = 0; i < innerVertices.length; i += 3) {
      points.push(
        new THREE.Vector3(innerVertices[i], innerVertices[i+1], innerVertices[i+2]),
        new THREE.Vector3(outerVertices[i], outerVertices[i+1], outerVertices[i+2])
      );
    }

    const connectionGeometry = new THREE.BufferGeometry().setFromPoints(points);
    const connectionMaterial = new THREE.LineBasicMaterial({ color: 0x666666 });
    const connections = new THREE.LineSegments(connectionGeometry, connectionMaterial);
    scene.add(connections);

    // animate
    function animate() {
      requestAnimationFrame(animate);
      innerCube.rotation.x += 0.01;
      innerCube.rotation.y += 0.01;
      outerCube.rotation.x += 0.01;
      outerCube.rotation.y += 0.01;
      connections.rotation.x += 0.01;
      connections.rotation.y += 0.01;
      renderer.render(scene, camera);
    }
    animate();
  </script>