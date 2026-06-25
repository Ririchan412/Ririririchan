
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Riri chan · Hias Kue 3D</title>
  <style>
    body { margin: 0; overflow: hidden; font-family: 'Segoe UI', 'Quicksand', sans-serif; background: #1a1420; }
    #title {
      position: absolute;
      top: 18px;
      left: 0;
      width: 100%;
      text-align: center;
      z-index: 30;
      pointer-events: none;
      color: #ffe8f0;
      text-shadow: 0 0 40px #ff66aa, 0 4px 20px rgba(0,0,0,0.9);
      font-size: 2.4rem;
      font-weight: 700;
      letter-spacing: 8px;
      background: linear-gradient(180deg, rgba(30,15,30,0.8) 0%, transparent 100%);
      padding: 24px 0 50px 0;
      margin: 0;
    }
    #title small {
      display: block;
      font-size: 1rem;
      font-weight: 300;
      letter-spacing: 4px;
      opacity: 0.9;
      margin-top: -6px;
      color: #f0c8d8;
    }
    #ui {
      position: absolute;
      bottom: 30px;
      left: 0;
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      padding: 16px 12px;
      box-sizing: border-box;
      background: linear-gradient(0deg, rgba(0,0,0,0.8) 0%, transparent 100%);
      pointer-events: none;
      z-index: 40;
    }
    #ui > div {
      pointer-events: auto;
      background: rgba(40, 25, 45, 0.75);
      backdrop-filter: blur(14px);
      border-radius: 60px;
      padding: 10px 18px;
      border: 1px solid rgba(255, 200, 230, 0.15);
      box-shadow: 0 8px 32px rgba(0,0,0,0.6);
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: center;
      gap: 6px 14px;
    }
    #ui button {
      background: rgba(255, 200, 230, 0.08);
      border: 1px solid rgba(255, 180, 210, 0.2);
      color: #f5e0ed;
      padding: 8px 20px;
      border-radius: 40px;
      font-weight: 600;
      font-size: 0.85rem;
      letter-spacing: 0.5px;
      cursor: pointer;
      transition: all 0.2s;
      backdrop-filter: blur(4px);
      text-shadow: 0 1px 8px rgba(0,0,0,0.6);
      box-shadow: 0 2px 10px rgba(0,0,0,0.3);
    }
    #ui button:hover { background: rgba(255, 120, 200, 0.2); border-color: #ff99cc; transform: scale(1.04); }
    #ui button:active { transform: scale(0.94); }
    #ui .label {
      color: #dbb0cc;
      font-size: 0.75rem;
      font-weight: 400;
      letter-spacing: 1px;
      text-transform: uppercase;
      margin-right: 2px;
    }
    #ui .color-option {
      display: inline-block;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      border: 2px solid rgba(255,255,255,0.15);
      cursor: pointer;
      transition: 0.15s;
      box-shadow: 0 0 12px rgba(0,0,0,0.4);
    }
    #ui .color-option:hover { transform: scale(1.15); border-color: white; }
    #ui .color-option.active { border-color: #ffb3e0; box-shadow: 0 0 24px #ff66aa; }
    #info {
      position: absolute;
      bottom: 110px;
      left: 0;
      width: 100%;
      text-align: center;
      color: rgba(255,220,240,0.5);
      font-size: 0.8rem;
      letter-spacing: 2px;
      pointer-events: none;
      z-index: 25;
      text-shadow: 0 2px 12px rgba(0,0,0,0.8);
    }
    @media (max-width: 720px) {
      #title { font-size: 1.8rem; letter-spacing: 4px; }
      #ui > div { padding: 8px 10px; gap: 4px 8px; border-radius: 40px; }
      #ui button { font-size: 0.7rem; padding: 5px 12px; }
      #ui .color-option { width: 24px; height: 24px; }
      #info { bottom: 100px; font-size: 0.65rem; }
    }
  </style>
</head>
<body>

  <div id="title">🎂 Riri chan <small>hias kue 3D</small></div>
  <div id="info">✨ klik & seret untuk putar · klik kue untuk tambah hiasan ✨</div>

  <div id="ui">
    <div>
      <span class="label">🧁 hiasan</span>
      <button id="deco-strawberry">🍓</button>
      <button id="deco-cherry">🍒</button>
      <button id="deco-star">⭐</button>
      <button id="deco-flower">🌸</button>
      <button id="deco-candle">🕯️</button>
      <span class="label" style="margin-left:4px;">🎨 warna</span>
      <span class="color-option active" style="background:#f8c8d8;" data-color="#f8c8d8"></span>
      <span class="color-option" style="background:#e8b0c0;" data-color="#e8b0c0"></span>
      <span class="color-option" style="background:#d4a0b0;" data-color="#d4a0b0"></span>
      <span class="color-option" style="background:#c090a8;" data-color="#c090a8"></span>
      <span class="color-option" style="background:#a88098;" data-color="#a88098"></span>
      <button id="reset-cake" style="background:rgba(200,70,70,0.15); border-color:rgba(255,100,100,0.3);">↺ reset</button>
    </div>
  </div>

  <script type="importmap">
    { "imports": { "three": "https://unpkg.com/three@0.128.0/build/three.module.js" } }
  </script>

  <script type="module">
    import * as THREE from 'three';
    import { OrbitControls } from 'https://unpkg.com/three@0.128.0/examples/jsm/controls/OrbitControls.js';

    // --- setup ---
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x1f1525);
    const camera = new THREE.PerspectiveCamera(40, window.innerWidth/window.innerHeight, 0.1, 30);
    camera.position.set(4, 3.2, 6);
    camera.lookAt(0, 1.2, 0);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.1;
    document.body.appendChild(renderer.domElement);

    // controls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.target.set(0, 1.3, 0);
    controls.enableDamping = true;
    controls.dampingFactor = 0.08;
    controls.autoRotate = false;
    controls.maxPolarAngle = Math.PI / 2.1;
    controls.minDistance = 2.5;
    controls.maxDistance = 10;
    controls.update();

    // --- lights ---
    const ambient = new THREE.AmbientLight(0x554066, 0.6);
    scene.add(ambient);
    const main = new THREE.DirectionalLight(0xffe8f0, 1.0);
    main.position.set(5, 10, 7);
    main.castShadow = true;
    main.shadow.mapSize.width = 1024;
    main.shadow.mapSize.height = 1024;
    scene.add(main);
    const fill = new THREE.DirectionalLight(0x99ccff, 0.5);
    fill.position.set(-4, 2, -5);
    scene.add(fill);
    const rim = new THREE.DirectionalLight(0xff99bb, 0.4);
    rim.position.set(0, -1, 7);
    scene.add(rim);
    const back = new THREE.PointLight(0xff66aa, 0.3);
    back.position.set(-2, 1, -6);
    scene.add(back);

    // ground / meja
    const tableMat = new THREE.MeshStandardMaterial({ color: 0x2a1f30, roughness: 0.8, metalness: 0.0 });
    const table = new THREE.Mesh(new THREE.CylinderGeometry(3.5, 3.8, 0.4, 32), tableMat);
    table.position.y = -0.2;
    table.receiveShadow = true;
    table.castShadow = true;
    scene.add(table);
    
    const grid = new THREE.GridHelper(6, 20, 0x8855aa, 0x553366);
    grid.position.y = 0.0;
    scene.add(grid);

    // --- KUE UTAMA ---
    const cakeGroup = new THREE.Group();
    scene.add(cakeGroup);

    // warna dasar kue (akan berubah)
    let cakeColor = 0xf8c8d8;
    const cakeMat = new THREE.MeshStandardMaterial({ color: cakeColor, roughness: 0.5, metalness: 0.0 });

    // layer bawah
    const layer1 = new THREE.Mesh(new THREE.CylinderGeometry(1.6, 1.7, 0.8, 24), cakeMat);
    layer1.position.y = 0.4;
    layer1.castShadow = true;
    layer1.receiveShadow = true;
    cakeGroup.add(layer1);

    // layer tengah
    const layer2 = new THREE.Mesh(new THREE.CylinderGeometry(1.3, 1.4, 0.7, 24), cakeMat);
    layer2.position.y = 1.1;
    layer2.castShadow = true;
    layer2.receiveShadow = true;
    cakeGroup.add(layer2);

    // layer atas
    const layer3 = new THREE.Mesh(new THREE.CylinderGeometry(1.0, 1.1, 0.6, 24), cakeMat);
    layer3.position.y = 1.75;
    layer3.castShadow = true;
    layer3.receiveShadow = true;
    cakeGroup.add(layer3);

    // krim / frosting (dekoratif)
    const creamMat = new THREE.MeshStandardMaterial({ color: 0xf5e0e8, roughness: 0.3, metalness: 0.0 });
    for (let i = 0; i < 16; i++) {
      const angle = (i / 16) * Math.PI * 2;
      const r = 1.3 + Math.random() * 0.2;
      const cream = new THREE.Mesh(new THREE.SphereGeometry(0.12, 6, 6), creamMat);
      cream.position.set(Math.cos(angle)*r, 0.8, Math.sin(angle)*r);
      cream.castShadow = true;
      cakeGroup.add(cream);
    }
    for (let i = 0; i < 12; i++) {
      const angle = (i / 12) * Math.PI * 2 + 0.2;
      const r = 1.0 + Math.random() * 0.2;
      const cream = new THREE.Mesh(new THREE.SphereGeometry(0.1, 6, 6), creamMat);
      cream.position.set(Math.cos(angle)*r, 1.45, Math.sin(angle)*r);
      cream.castShadow = true;
      cakeGroup.add(cream);
    }

    // --- HIASAN (decorations) ---
    const decoGroup = new THREE.Group();
    cakeGroup.add(decoGroup);

    // daftar hiasan yang sudah ditempel
    let decorations = [];

    // fungsi untuk menambah hiasan
    function addDecoration(type, color = 0xff66aa) {
      const pos = getRandomPositionOnCake();
      let mesh;
      const mat = new THREE.MeshStandardMaterial({ color: color, roughness: 0.3, metalness: 0.1, emissive: new THREE.Color(color).multiplyScalar(0.1) });
      
      if (type === 'strawberry') {
        const group = new THREE.Group();
        const body = new THREE.Mesh(new THREE.SphereGeometry(0.18, 8, 8), new THREE.MeshStandardMaterial({ color: 0xff3344, roughness: 0.3 }));
        body.scale.set(1, 0.85, 1);
        body.position.y = 0.05;
        group.add(body);
        const top = new THREE.Mesh(new THREE.SphereGeometry(0.04, 6, 6), new THREE.MeshStandardMaterial({ color: 0x22aa44 }));
        top.position.y = 0.22;
        group.add(top);
        const leaf = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.08, 4), new THREE.MeshStandardMaterial({ color: 0x33aa55 }));
        leaf.position.y = 0.26;
        leaf.rotation.y = 0.4;
        group.add(leaf);
        mesh = group;
      } else if (type === 'cherry') {
        const group = new THREE.Group();
        const body = new THREE.Mesh(new THREE.SphereGeometry(0.16, 8, 8), new THREE.MeshStandardMaterial({ color: 0xcc2244, roughness: 0.2 }));
        group.add(body);
        const stem = new THREE.Mesh(new THREE.CylinderGeometry(0.01, 0.01, 0.15, 4), new THREE.MeshStandardMaterial({ color: 0x44aa55 }));
        stem.position.y = 0.2;
        stem.rotation.x = 0.3;
        stem.rotation.z = 0.4;
        group.add(stem);
        const leaf2 = new THREE.Mesh(new THREE.ConeGeometry(0.04, 0.06, 4), new THREE.MeshStandardMaterial({ color: 0x55bb66 }));
        leaf2.position.set(0.03, 0.28, 0);
        group.add(leaf2);
        mesh = group;
      } else if (type === 'star') {
        const star = new THREE.Mesh(new THREE.OctahedronGeometry(0.16), new THREE.MeshStandardMaterial({ color: 0xffdd44, emissive: 0x442200, emissiveIntensity: 0.2 }));
        star.rotation.x = 0.3;
        star.rotation.y = 0.5;
        mesh = star;
      } else if (type === 'flower') {
        const group = new THREE.Group();
        const center = new THREE.Mesh(new THREE.SphereGeometry(0.05, 6, 6), new THREE.MeshStandardMaterial({ color: 0xffdd44 }));
        group.add(center);
        for (let i = 0; i < 5; i++) {
          const angle = (i / 5) * Math.PI * 2;
          const petal = new THREE.Mesh(new THREE.SphereGeometry(0.07, 6, 6), new THREE.MeshStandardMaterial({ color: 0xff88aa, roughness: 0.2 }));
          petal.position.set(Math.cos(angle)*0.12, 0, Math.sin(angle)*0.12);
          petal.scale.set(1.2, 0.6, 1.2);
          group.add(petal);
        }
        mesh = group;
      } else if (type === 'candle') {
        const group = new THREE.Group();
        const candle = new THREE.Mesh(new THREE.CylinderGeometry(0.04, 0.05, 0.25, 8), new THREE.MeshStandardMaterial({ color: 0xffeedd, emissive: 0x442200, emissiveIntensity: 0.1 }));
        candle.position.y = 0.12;
        group.add(candle);
        const flame = new THREE.Mesh(new THREE.SphereGeometry(0.04, 6, 6), new THREE.MeshStandardMaterial({ color: 0xff6600, emissive: 0xff4400, emissiveIntensity: 0.5 }));
        flame.position.y = 0.28;
        flame.scale.set(0.7, 1.2, 0.7);
        group.add(flame);
        mesh = group;
      }

      if (mesh) {
        mesh.position.copy(pos);
        // sedikit random rotasi
        mesh.rotation.y = Math.random() * Math.PI * 2;
        mesh.castShadow = true;
        decoGroup.add(mesh);
        decorations.push({ mesh, type, pos });
        return mesh;
      }
      return null;
    }

    function getRandomPositionOnCake() {
      // ambil posisi di atas lapisan teratas
      const radius = 0.5 + Math.random() * 0.6;
      const angle = Math.random() * Math.PI * 2;
      const x = Math.cos(angle) * radius;
      const z = Math.sin(angle) * radius;
      const y = 2.0 + Math.random() * 0.25;
      return new THREE.Vector3(x, y, z);
    }

    // tambahkan beberapa hiasan awal
    addDecoration('strawberry', 0xff4466);
    addDecoration('cherry', 0xcc2244);
    addDecoration('star', 0xffdd44);
    addDecoration('flower', 0xff88aa);
    addDecoration('candle', 0xffeedd);

    // --- event: klik pada kue untuk menambah hiasan ---
    const raycaster = new THREE.Raycaster();
    const mouse = new THREE.Vector2();

    function onCakeClick(event) {
      // hitung posisi mouse
      const rect = renderer.domElement.getBoundingClientRect();
      let clientX, clientY;
      if (event.touches) {
        clientX = event.touches[0].clientX;
        clientY = event.touches[0].clientY;
        event.preventDefault();
      } else {
        clientX = event.clientX;
        clientY = event.clientY;
      }
      mouse.x = ((clientX - rect.left) / rect.width) * 2 - 1;
      mouse.y = -((clientY - rect.top) / rect.height) * 2 + 1;

      raycaster.setFromCamera(mouse, camera);
      
      // cek tabrakan dengan layer kue
      const meshes = [];
      cakeGroup.children.forEach(child => {
        if (child.isMesh) meshes.push(child);
      });
      const intersects = raycaster.intersectObjects(meshes);
      
      if (intersects.length > 0) {
        // tambah hiasan acak
        const types = ['strawberry', 'cherry', 'star', 'flower', 'candle'];
        const type = types[Math.floor(Math.random() * types.length)];
        const colors = [0xff4466, 0xcc2244, 0xffdd44, 0xff88aa, 0xffeedd, 0x66bbff, 0xaa88ff];
        const color = colors[Math.floor(Math.random() * colors.length)];
        addDecoration(type, color);
      }
    }

    renderer.domElement.addEventListener('click', onCakeClick);
    renderer.domElement.addEventListener('touchstart', onCakeClick, { passive: false });

    // --- UI events ---
    document.getElementById('deco-strawberry').addEventListener('click', () => addDecoration('strawberry', 0xff4466));
    document.getElementById('deco-cherry').addEventListener('click', () => addDecoration('cherry', 0xcc2244));
    document.getElementById('deco-star').addEventListener('click', () => addDecoration('star', 0xffdd44));
    document.getElementById('deco-flower').addEventListener('click', () => addDecoration('flower', 0xff88aa));
    document.getElementById('deco-candle').addEventListener('click', () => addDecoration('candle', 0xffeedd));

    // reset
    document.getElementById('reset-cake').addEventListener('click', () => {
      while(decoGroup.children.length) decoGroup.remove(decoGroup.children[0]);
      decorations = [];
      // tambah beberapa default
      addDecoration('strawberry', 0xff4466);
      addDecoration('cherry', 0xcc2244);
      addDecoration('star', 0xffdd44);
      addDecoration('flower', 0xff88aa);
      addDecoration('candle', 0xffeedd);
    });

    // warna kue
    document.querySelectorAll('.color-option').forEach(el => {
      el.addEventListener('click', () => {
        document.querySelectorAll('.color-option').forEach(c => c.classList.remove('active'));
        el.classList.add('active');
        const hex = el.dataset.color;
        cakeColor = parseInt(hex.slice(1), 16);
        cakeMat.color.setHex(cakeColor);
        // update semua layer
        cakeGroup.children.forEach(child => {
          if (child.isMesh && child.material === cakeMat) {
            child.material.color.setHex(cakeColor);
          }
        });
      });
    });

    // --- animasi ---
    function animate() {
      requestAnimationFrame(animate);
      controls.update();
      
      // animasi lilin (nyala)
      decoGroup.children.forEach(child => {
        if (child.type === 'Group') {
          child.children.forEach(sub => {
            if (sub.isMesh && sub.material && sub.material.emissive) {
              if (sub.material.color.getHex() === 0xff6600) {
                const flicker = 0.4 + Math.random() * 0.6;
                sub.scale.y = 0.8 + Math.random() * 0.5;
                sub.material.emissiveIntensity = 0.3 + Math.random() * 0.5;
              }
            }
          });
        }
      });
      
      renderer.render(scene, camera);
    }
    animate();

    // resize
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });

    console.log('🎂 Riri chan — Hias Kue 3D! Klik kue untuk tambah hiasan.');
  </script>
</body>
</html>
