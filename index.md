---
layout: page
title: Home
---

# Welcome to My Personal Website


  <div class="model-box">
    <div class="hud">Tesseract • Rotating in 4D (XW & YZ)</div>
    <canvas id="modelCanvas"></canvas>
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
      font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
    }

    .model-box {
      width: 320px;
      height: 320px;
      position: relative;
      overflow: visible; /* allow subtle spillover */
      border: 1px solid #ddd;
      border-radius: 10px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.06);
      background: #fdfdfd;
    }

    canvas {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 400px;   /* visually larger than the box */
      height: 400px;
      display: block;
      pointer-events: none; /* don't block interactions nearby */
    }

    .hud {
      position: absolute;
      left: 10px;
      top: 10px;
      font-size: 12px;
      padding: 6px 8px;
      background: rgba(0,0,0,0.06);
      border-radius: 8px;
      color: #111;
      user-select: none;
    }
</style>

<script type="module">
    import * as THREE from "https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.module.js";

    // --- Renderer / Scene / Camera ---
    const canvas = document.getElementById("modelCanvas");
    const renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true });
    // IMPORTANT: setSize must match the CSS px size above (400x400)
    renderer.setSize(400, 400, false);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 2));
    renderer.setClearColor(0xfdfdfd, 1);

    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, 1, 0.01, 100);
    camera.position.set(0, 0, 5);

    // --- Tesseract (4D hypercube) setup ---
    // 16 vertices: all combinations of (+/-1, +/-1, +/-1, +/-1)
    const verts4D = [];
    for (let w of [-1, 1]) {
      for (let z of [-1, 1]) {
        for (let y of [-1, 1]) {
          for (let x of [-1, 1]) {
            verts4D.push([x, y, z, w]);
          }
        }
      }
    }

    // Build edge list: connect vertices that differ by exactly one coordinate
    const edges = [];
    for (let i = 0; i < verts4D.length; i++) {
      for (let j = i + 1; j < verts4D.length; j++) {
        const a = verts4D[i], b = verts4D[j];
        let diff = 0;
        for (let k = 0; k < 4; k++) if (a[k] !== b[k]) diff++;
        if (diff === 1) edges.push([i, j]);
      }
    }
    // edges should be 32 pairs for a tesseract

    // 4D rotation helpers: rotate in a coordinate plane (ab) with angle t
    function rotate4D(v, a, b, t) {
      const s = Math.sin(t), c = Math.cos(t);
      const out = v.slice();
      const va = v[a], vb = v[b];
      out[a] = va * c - vb * s;
      out[b] = va * s + vb * c;
      return out;
    }

    // Project 4D -> 3D with perspective on W
    // The larger 'perspD' is, the weaker the 4D perspective effect.
    function project4Dto3D(v4, perspD = 3.0) {
      const [x, y, z, w] = v4;
      const denom = (perspD - w);
      const s = 1.0 / (denom !== 0 ? denom : 1e-6);
      return [x * s, y * s, z * s];
    }

    // Create line geometry (will update each frame)
    const linePositions = new Float32Array(edges.length * 2 * 3);
    const lineGeom = new THREE.BufferGeometry();
    lineGeom.setAttribute("position", new THREE.BufferAttribute(linePositions, 3));

    const lineMat = new THREE.LineBasicMaterial({ color: 0x444444, linewidth: 1 });
    const wire = new THREE.LineSegments(lineGeom, lineMat);
    wire.scale.set(1.4, 1.4, 1.4); // overall scale so it "breathes" out a bit
    scene.add(wire);

    // Optional soft light to give scene some depth (does not affect lines, but helps if you add meshes)
    scene.add(new THREE.AmbientLight(0xffffff, 0.2));

    // Animation: rotate in XW and YZ planes for a pleasing effect
    let start = performance.now();
    function animate() {
      const t = (performance.now() - start) * 0.001;
      const angle1 = t * 0.7;  // XW plane
      const angle2 = t * 0.9;  // YZ plane

      // Compute transformed 3D points
      const pts3 = [];
      for (let v of verts4D) {
        let r = v;
        r = rotate4D(r, 0, 3, angle1); // rotate in (X=0, W=3)
        r = rotate4D(r, 1, 2, angle2); // rotate in (Y=1, Z=2)
        const p = project4Dto3D(r, 3.2); // tweak 4D perspective depth
        pts3.push(p);
      }

      // Update line segment positions
      let idx = 0;
      for (let e of edges) {
        const a = pts3[e[0]];
        const b = pts3[e[1]];
        linePositions[idx++] = a[0];
        linePositions[idx++] = a[1];
        linePositions[idx++] = a[2];
        linePositions[idx++] = b[0];
        linePositions[idx++] = b[1];
        linePositions[idx++] = b[2];
      }
      lineGeom.attributes.position.needsUpdate = true;

      // Gentle spin in 3D space so the projection also turns
      wire.rotation.y = t * 0.3;
      wire.rotation.x = Math.sin(t * 0.5) * 0.1;

      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }
    animate();

    // (Optional) If you ever want to make the container responsive:
    // just update renderer size here. For this fixed 320 box + 400 canvas demo,
    // we keep it static on purpose to preserve the subtle overflow.
    // window.addEventListener("resize", () => {
    //   renderer.setSize(400, 400, false);
    //   camera.aspect = 1;
    //   camera.updateProjectionMatrix();
    // });
  </script>