<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover"/>
<title>Rudra “Rudi” Pratap Singh — Mission: RUDI</title>
<meta name="description" content="Rudra Pratap Singh — technologist, maker, taekwondo black belt. Portfolio: projects, gallery, resume." />
<link rel="icon" href="assets/favicon.png" />

<!-- Fonts: system + crisp display -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;900&display=swap" rel="stylesheet">

<!-- THREE.js + GSAP -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

<style>
  :root{
    --bg-1: #0b1320;
    --bg-2: #071025;
    --glass: rgba(255,255,255,0.04);
    --muted: #9fbfcf;
    --accent-a: #00ffe0;
    --accent-b: #9b00ff;
    --card-radius: 18px;
    --maxw: 1200px;
    --ease: cubic-bezier(.2,.9,.3,1);
    color-scheme: dark;
  }

  /* Reset */
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;color:#e6f7ff}
  body{background:linear-gradient(180deg,var(--bg-1),var(--bg-2)); overflow-y:auto; -webkit-font-smoothing:antialiased}

  /* Full-screen WebGL canvas sits below content */
  #bg-gl { position: fixed; inset:0; z-index:0; width:100%; height:100%; display:block; }

  /* Top nav */
  .nav {
    position:fixed; top:18px; left:18px; right:18px; z-index:30; display:flex; justify-content:space-between; align-items:center;
    pointer-events:auto;
  }
  .brand { display:flex; gap:12px; align-items:center; }
  .brand .logo {
    width:46px; height:46px; border-radius:12px; background:linear-gradient(135deg,var(--accent-a),var(--accent-b));
    display:flex; align-items:center; justify-content:center; font-weight:900; color:#001; box-shadow:0 8px 30px rgba(0,0,0,0.6);
  }
  nav a { color:rgba(255,255,255,0.86); text-decoration:none; font-weight:700; padding:8px 12px; border-radius:10px; margin-left:8px; opacity:0.95 }
  nav a.secondary{ background:transparent; border:1px solid rgba(255,255,255,0.04) }
  nav a.primary{ background:linear-gradient(90deg,var(--accent-a),var(--accent-b)); color:#001; box-shadow:0 10px 30px rgba(155,0,255,0.12) }

  /* Stage / Layout */
  .stage { position:relative; z-index:10; min-height:100vh; display:flex; align-items:center; justify-content:center; padding:120px 20px 80px; }
  .container { width:min(var(--maxw),96%); margin:0 auto; display:grid; grid-template-columns:420px 1fr; gap:28px; align-items:start; }

  /* Left: avatar column */
  .left-card { background:var(--glass); border-radius:var(--card-radius); padding:22px; border:1px solid rgba(255,255,255,0.03); backdrop-filter: blur(8px); box-shadow:0 20px 80px rgba(2,6,23,0.6) ; display:flex; flex-direction:column; gap:18px; align-items:center; }
  .avatar-wrap { position:relative; width:320px; height:320px; border-radius:20px; overflow:hidden; background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(0,0,0,0.04)); display:flex; align-items:center; justify-content:center; }
  .avatar { width:100%; height:100%; object-fit:cover; display:block; transform-origin:center; transition:transform 0.9s var(--ease); }
  .avatar-ring { position:absolute; inset:8px; border-radius:16px; pointer-events:none; box-shadow:0 0 80px rgba(0,255,224,0.06); border:1px solid rgba(255,255,255,0.02); background:linear-gradient(180deg, rgba(0,255,224,0.03), rgba(155,0,255,0.03)); mix-blend-mode:screen; }

  .info { text-align:center; padding:8px 6px; }
  .title { font-size:28px; font-weight:900; letter-spacing:-0.5px; margin-bottom:6px; color:linear-gradient(90deg,var(--accent-a),var(--accent-b)); }
  .subtitle { color:var(--muted); margin-bottom:8px; font-weight:600 }

  .stat-grid { display:flex; gap:10px; flex-wrap:wrap; justify-content:center; }
  .stat { background:rgba(255,255,255,0.02); padding:8px 12px; border-radius:10px; font-weight:700; color:#dffcff; border:1px solid rgba(255,255,255,0.02) }

  .control-row { display:flex; gap:10px; margin-top:12px; }

  /* Right: content column */
  .right { color:#dffcff; }
  .hero { margin-bottom:12px; }
  .hero h1 { font-size:48px; margin:0 0 6px; line-height:1; font-weight:900; letter-spacing:-1px; -webkit-background-clip:text; background:linear-gradient(90deg,var(--accent-a),var(--accent-b)); color:transparent; -webkit-text-fill-color:transparent }
  .hero p { color: #cfeef0; font-size:18px; max-width:720px; margin:0 0 12px; }
  .badges { display:flex; gap:10px; flex-wrap:wrap; margin-bottom:12px; }
  .badge { padding:8px 12px; border-radius:999px; background:rgba(255,255,255,0.03); color:#bffcf3; font-weight:700; border:1px solid rgba(255,255,255,0.025) }

  /* Projects grid / gallery */
  .projects { display:grid; grid-template-columns:repeat(2, 1fr); gap:18px; margin-top:18px; }
  .proj { background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:16px; border-radius:12px; border:1px solid rgba(255,255,255,0.03); box-shadow:0 20px 60px rgba(2,6,23,0.6); cursor:pointer; transition:transform 0.45s var(--ease), box-shadow 0.45s var(--ease) }
  .proj:hover { transform:translateY(-8px); box-shadow:0 30px 90px rgba(0,0,0,0.6) }
  .proj img { width:100%; height:180px; object-fit:cover; border-radius:8px; display:block; }

  /* Footer */
  footer { margin-top:48px; text-align:center; color:rgba(255,255,255,0.52); font-size:13px; padding-bottom:40px; }

  /* Modal viewer */
  .modal { position:fixed; inset:0; display:flex; align-items:center; justify-content:center; z-index:60; background:rgba(0,0,0,0.6); }
  .modal .box { width:min(1000px,96%); background:linear-gradient(180deg,#021018,#000814); border-radius:14px; padding:18px; border:1px solid rgba(255,255,255,0.04); box-shadow:0 30px 120px rgba(0,0,0,0.8) }
  .modal img{ width:100%; height:auto; border-radius:8px; display:block; }

  /* small screens */
  @media (max-width:980px){
    .container{ grid-template-columns: 1fr; padding: 0 12px; }
    .hero h1 { font-size:34px; }
    .projects{ grid-template-columns:1fr; }
    .avatar-wrap{ width:260px; height:260px }
  }

  /* utility */
  .hidden{display:none}
  .sr-only{position:absolute;left:-9999px}
</style>
</head>
<body>

<!-- WebGL background canvas (shader + particles) -->
<canvas id="bg-gl" aria-hidden="true"></canvas>

<!-- Top nav -->
<div class="nav" role="navigation" aria-label="Main navigation">
  <div class="brand">
    <div class="logo">R</div>
    <div style="color:#dffcff;font-weight:700">RUDI — Mission Control</div>
  </div>
  <nav>
    <a href="#projects" class="secondary">Projects</a>
    <a href="#gallery" class="secondary">Gallery</a>
    <a id="download-resume" class="primary" href="assets/resume.pdf" download>Resume</a>
  </nav>
</div>

<!-- Stage -->
<section class="stage" id="home">
  <div class="container" role="main">

    <!-- LEFT -->
    <aside class="left-card" aria-label="Profile">
      <div class="avatar-wrap" id="avatarWrap">
        <!-- Replace 'assets/your-photo.jpg' with a high-quality square photo -->
        <img id="avatar" class="avatar" src="assets/rudi_avatar.svg" alt="Rudra Pratap Singh — Portrait" loading="lazy"
             srcset="assets/your-photo-400.jpg 400w, assets/your-photo-800.jpg 800w, assets/your-photo-1200.jpg 1200w"
             sizes="(max-width:980px) 80vw, 320px" onerror="this.src='assets/rudi_avatar.svg'">
        <div class="avatar-ring" aria-hidden="true"></div>
      </div>

      <div class="info">
        <div class="title">Rudra "Rudi" Pratap Singh</div>
        <div class="subtitle">Digital Samurai • Maker • Taekwondo Black Belt</div>

        <div class="stat-grid" role="list">
          <div class="stat" role="listitem">DOB: 23 Jun 2006</div>
          <div class="stat" role="listitem">Raipur → Varanasi → SRM</div>
          <div class="stat" role="listitem">Boards: 70%</div>
        </div>

        <div class="control-row">
          <button id="contactBtn" class="proj">Contact</button>
          <a id="resumeBtn" class="proj" href="assets/resume.pdf" download>Download Resume</a>
        </div>
      </div>
    </aside>

    <!-- RIGHT -->
    <main class="right">
      <div class="hero" aria-labelledby="hero-title">
        <h1 id="hero-title">Systems, Speed, & Stories — Built</h1>
        <p>A maker of mechanisms and experiences. From model rocketry to embedded controllers and public demos — I build reliable systems, lead teams, and pivot hard when something matters.</p>
        <div class="badges">
          <div class="badge">Embedded & Electronics</div>
          <div class="badge">WebGL & Visualization</div>
          <div class="badge">Leadership & Events</div>
        </div>
      </div>

      <section id="projects" aria-labelledby="projects-title">
        <h2 id="projects-title" style="margin-bottom:12px">Featured Projects</h2>
        <div class="projects" id="projectsGrid">
          <!-- Replace images & links -->
          <article class="proj" tabindex="0" data-title="GSLV Mk III — Scale Model" data-desc="Led a team to design & build a working scale model; public coverage." data-img="assets/photo1.jpg">
            <img src="assets/photo1.jpg" alt="GSLV model" loading="lazy"/>
            <h3 style="margin:10px 0 6px">GSLV Mk III — Scale Model</h3>
            <p style="color:var(--muted)">Lead: hardware design, integration, public demos.</p>
          </article>

          <article class="proj" tabindex="0" data-title="Embedded Flight Controller" data-desc="Controller prototype for brushless motors and sensors." data-img="assets/photo2.jpg">
            <img src="assets/photo2.jpg" alt="Flight controller" loading="lazy"/>
            <h3 style="margin:10px 0 6px">Embedded Flight Controller</h3>
            <p style="color:var(--muted)">Sensor fusion, safety interlocks, PWM motor control.</p>
          </article>

          <article class="proj" tabindex="0" data-title="WebGL Visualizer" data-desc="3D visualization suite for trajectories and telemetry." data-img="assets/photo3.jpg">
            <img src="assets/photo3.jpg" alt="WebGL" loading="lazy"/>
            <h3 style="margin:10px 0 6px">WebGL Telemetry Visualizer</h3>
            <p style="color:var(--muted)">Realtime trajectory visualization with interactive controls.</p>
          </article>

          <article class="proj" tabindex="0" data-title="Taekwondo App" data-desc="Training tracker & performance timelines." data-img="assets/photo4.jpg">
            <img src="assets/photo4.jpg" alt="Taekwondo" loading="lazy"/>
            <h3 style="margin:10px 0 6px">Taekwondo Demo App</h3>
            <p style="color:var(--muted)">Progress tracking and achievements UI.</p>
          </article>
        </div>
      </section>

      <section id="gallery" style="margin-top:22px">
        <h2 style="margin-bottom:12px">Gallery & Press</h2>
        <div style="display:flex;gap:12px;flex-wrap:wrap">
          <img src="assets/photo1.jpg" alt="gallery-1" width="220" height="140" loading="lazy" style="border-radius:8px"/>
          <img src="assets/photo2.jpg" alt="gallery-2" width="220" height="140" loading="lazy" style="border-radius:8px"/>
          <img src="assets/photo3.jpg" alt="gallery-3" width="220" height="140" loading="lazy" style="border-radius:8px"/>
          <img src="assets/photo4.jpg" alt="gallery-4" width="220" height="140" loading="lazy" style="border-radius:8px"/>
        </div>
      </section>

      <section id="contact" style="margin-top:30px">
        <h2 style="margin-bottom:8px">Contact</h2>
        <p style="color:var(--muted)">Ready to collaborate? Send a short message.</p>

        <!-- FORMSPREE: replace /f/your-id with your Formspree endpoint -->
        <form id="contactForm" action="https://formspree.io/f/your-id" method="POST" style="margin-top:12px;display:grid;gap:10px;max-width:640px">
          <input name="name" type="text" placeholder="Your name" required style="padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit"/>
          <input name="email" type="email" placeholder="Your email" required style="padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit"/>
          <textarea name="message" rows="5" placeholder="Message" required style="padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit"></textarea>
          <div style="display:flex;gap:10px">
            <button class="primary" type="submit" style="padding:10px 14px;border-radius:10px">Send</button>
            <button id="copyEmail" type="button" style="padding:10px 14px;border-radius:10px;background:transparent;border:1px solid rgba(255,255,255,0.04)">Copy email</button>
          </div>
        </form>
      </section>

    </main>

  </div>
</section>

<footer role="contentinfo">
  © 2025 Rudra "Rudi" Pratap Singh — Built for impact.
</footer>

<!-- Project modal -->
<div id="projModal" class="hidden" aria-hidden="true"></div>

<!-- Minimal service worker registration (PWA ready) -->
<script>
  if ('serviceWorker' in navigator) {
    try {
      navigator.serviceWorker.register('/sw.js').catch(()=>{/* ignore for now */});
    } catch(e){}
  }
</script>

<script>
/* ---------------------------
   Initialization & utilities
   --------------------------- */
gsap.registerPlugin(ScrollTrigger);
const clamp = (v,a,b)=>Math.max(a,Math.min(b,v));

/* ---------------------------
   Shader background (Three.js)
   - Nebula + subtle stars + animated noise
   --------------------------- */
(function initGL(){
  const canvas = document.getElementById('bg-gl');
  const renderer = new THREE.WebGLRenderer({ canvas: canvas, antialias:false, alpha:true, powerPreference:'high-performance' });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio,2));
  renderer.setSize(innerWidth, innerHeight);

  const scene = new THREE.Scene();
  const camera = new THREE.OrthographicCamera(-innerWidth/2, innerWidth/2, innerHeight/2, -innerHeight/2, 0, 10);
  camera.position.z = 1;

  // plane with fragment shader (simple procedural nebula + stars)
  const planeGeo = new THREE.PlaneGeometry(innerWidth, innerHeight);
  const frag = `
    precision highp float;
    uniform float t;
    uniform vec2 res;
    // random & noise helpers
    float hash(vec2 p){ return fract(sin(dot(p,vec2(127.1,311.7)))*43758.5453123); }
    float noise(vec2 p){
      vec2 i=floor(p); vec2 f=fract(p);
      float a=hash(i), b=hash(i+vec2(1.0,0.0)), c=hash(i+vec2(0.0,1.0)), d=hash(i+vec2(1.0,1.0));
      vec2 u=f*f*(3.0-2.0*f);
      return mix(a, b, u.x) + (c-a)*u.y*(1.0-u.x) + (d-b)*u.x*u.y;
    }
    void main(){
      vec2 uv = gl_FragCoord.xy / res.xy;
      vec2 p = uv * 2.0 - 1.0;
      vec2 q = p * vec2(res.x/res.y,1.0);

      // layered noise
      float n = 0.0;
      n += 0.6 * noise(q * 1.2 + t*0.02);
      n += 0.3 * noise(q * 3.0 - t*0.015);
      n += 0.12 * noise(q * 8.0 + t*0.01);

      // colorize
      vec3 col = mix(vec3(0.01,0.02,0.06), vec3(0.02,0.06,0.08), n);
      // nebula highlight
      float glow = smoothstep(0.45, 0.8, n);
      col += vec3(0.0,0.6,0.45) * glow * 0.6;

      // stars
      float stars = step(0.9965, hash(gl_FragCoord.xy * 0.5 + floor(t*10.0)));
      col += vec3(stars);

      gl_FragColor = vec4(col, 1.0);
    }
  `;

  const material = new THREE.ShaderMaterial({
    fragmentShader: frag,
    uniforms: { t: { value: 0.0 }, res: { value: new THREE.Vector2(innerWidth, innerHeight) } },
    depthTest: false,
  });
  const mesh = new THREE.Mesh(planeGeo, material);
  scene.add(mesh);

  // subtle moving stars (points)
  const starsGeo = new THREE.BufferGeometry();
  const starCount = 600;
  const positions = new Float32Array(starCount*3);
  for(let i=0;i<starCount;i++){
    positions[i*3+0] = (Math.random()*2-1) * innerWidth;
    positions[i*3+1] = (Math.random()*2-1) * innerHeight;
    positions[i*3+2] = -50 * Math.random();
  }
  starsGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  const starMat = new THREE.PointsMaterial({ color:0xffffff, size:1.6, sizeAttenuation:true, opacity:0.9, transparent:true });
  const starPoints = new THREE.Points(starsGeo, starMat);
  scene.add(starPoints);

  function onResize(){
    renderer.setSize(innerWidth, innerHeight);
    mesh.geometry.dispose();
    mesh.geometry = new THREE.PlaneGeometry(innerWidth, innerHeight);
    camera.left = -innerWidth/2; camera.right = innerWidth/2; camera.top = innerHeight/2; camera.bottom = -innerHeight/2;
    camera.updateProjectionMatrix();
    material.uniforms.res.value.set(innerWidth, innerHeight);
  }
  window.addEventListener('resize', onResize);

  let t=0;
  function animate(){
    t += 0.016;
    material.uniforms.t.value = t;
    // drift stars slowly downward
    const pos = starsGeo.attributes.position.array;
    for(let i=0;i<starCount;i++){
      pos[i*3+1] -= 0.2 + (pos[i*3+2]*0.001);
      if(pos[i*3+1] < -innerHeight) pos[i*3+1] = innerHeight;
    }
    starsGeo.attributes.position.needsUpdate = true;

    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
  animate();

  // Keep references for other scripts (optional)
  window.__RUDI_GL = { renderer, scene, camera, mesh, material };
})();

/* ---------------------------
  3D SATELLITE (Three primitives) — interactive
  --------------------------- */
(function initSatellite(){
  const mount = document.createElement('div');
  mount.style.position='fixed'; mount.style.right='24px'; mount.style.bottom='24px'; mount.style.width='220px'; mount.style.height='220px'; mount.style.zIndex=40; mount.style.pointerEvents='auto';
  document.body.appendChild(mount);

  const scene = new THREE.Scene();
  const cam = new THREE.PerspectiveCamera(40, 1, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer({ alpha:true, antialias:true, powerPreference:'high-performance' });
  renderer.setSize(220,220); renderer.setPixelRatio(Math.min(window.devicePixelRatio,2));
  mount.appendChild(renderer.domElement);

  cam.position.z = 4;

  const g1 = new THREE.BoxGeometry(1.2,0.3,0.8);
  const g2 = new THREE.BoxGeometry(2.0,0.06,0.8);
  const mat = new THREE.MeshStandardMaterial({ color:0x0b1320, metalness:0.8, roughness:0.2, emissive:0x00ffe0, emissiveIntensity:0.02 });
  const core = new THREE.Mesh(g1, mat);
  scene.add(core);
  const wingL = new THREE.Mesh(g2, mat); wingL.position.set(-1.5,0,0); wingL.rotation.z = Math.PI*0.08; scene.add(wingL);
  const wingR = new THREE.Mesh(g2, mat); wingR.position.set(1.5,0,0); wingR.rotation.z = -Math.PI*0.08; scene.add(wingR);

  const point = new THREE.PointLight(0xffffff, 0.8); point.position.set(5,5,5); scene.add(point);
  const amb = new THREE.AmbientLight(0x224455, 0.8); scene.add(amb);

  // subtle orbit animation and pointer interactivity
  let mx=0,my=0;
  mount.addEventListener('mousemove', e=>{ const r=mount.getBoundingClientRect(); mx = (e.clientX - (r.left + r.width/2))/r.width; my = (e.clientY - (r.top + r.height/2))/r.height; });
  function tick(){
    core.rotation.y += 0.008;
    core.rotation.x += 0.003;
    wingL.rotation.y = 0.2*Math.sin(performance.now()*0.001);
    wingR.rotation.y = -0.2*Math.sin(performance.now()*0.001);
    // follow pointer
    cam.position.x += (mx*1.5 - cam.position.x) * 0.06;
    cam.position.y += (-my*1.5 - cam.position.y) * 0.06;
    cam.lookAt(0,0,0);
    renderer.render(scene, cam);
    requestAnimationFrame(tick);
  }
  tick();

  // accessible label
  mount.setAttribute('role','img'); mount.setAttribute('aria-label','Interactive satellite model');
})();

/* ---------------------------
  UX: entrance animations, scroll-trigger
  --------------------------- */
(function ui(){
  // fade-in content on load
  gsap.from('.left-card', { y: 20, opacity:0, duration:1.2, ease:'power3.out' });
  gsap.from('.hero h1', { y: 10, opacity:0, duration:1.0, delay:0.2, ease:'power3.out' });
  gsap.from('.right p, .badge', { opacity:0, y:10, stagger:0.06, delay:0.3, duration:0.9 });

  // sections animate on scroll
  document.querySelectorAll('section, .proj').forEach(el=>{
    gsap.from(el, {
      scrollTrigger: { trigger: el, start: 'top 85%', toggleActions: 'play none none none' },
      y: 14, opacity: 0, duration: 0.9, ease: 'power3.out'
    });
  });

  // project open modal
  const modal = document.getElementById('projModal');
  function openProject(title, desc, img){
    modal.innerHTML = '';
    modal.classList.remove('hidden'); modal.style.display = 'flex';
    modal.setAttribute('aria-hidden','false');
    const box = document.createElement('div'); box.className='box';
    box.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px"><strong style="font-size:18px;color:#dffcff">${title}</strong><button id="closeProj" style="background:transparent;border:none;color:#dffcff;font-weight:800;font-size:16px">✕</button></div>
                     <img src="${img}" alt="${title}" loading="lazy" />
                     <p style="color:var(--muted);margin-top:8px">${desc}</p>`;
    modal.appendChild(box);
    document.getElementById('closeProj').addEventListener('click', closeModal);
    modal.addEventListener('click', (e)=>{ if(e.target===modal) closeModal(); });
  }
  function closeModal(){ modal.innerHTML=''; modal.classList.add('hidden'); modal.style.display='none'; modal.setAttribute('aria-hidden','true'); }

  document.querySelectorAll('.proj').forEach(p=>{
    p.addEventListener('click', ()=> openProject(p.dataset.title, p.dataset.desc, p.dataset.img));
    p.addEventListener('keypress', (e)=>{ if(e.key==='Enter') openProject(p.dataset.title, p.dataset.desc, p.dataset.img); });
  });

  // avatar hover subtle
  const avatar = document.getElementById('avatar');
  const avatarWrap = document.getElementById('avatarWrap');
  avatarWrap.addEventListener('mousemove', (e)=>{
    const r = avatarWrap.getBoundingClientRect();
    const dx = (e.clientX - r.left - r.width/2) / r.width;
    const dy = (e.clientY - r.top - r.height/2) / r.height;
    gsap.to(avatar, { x: dx*18, y: dy*12, rotation: dx*4, duration: 0.9, ease: 'power2.out' });
  });
  avatarWrap.addEventListener('mouseleave', ()=> gsap.to(avatar, { x:0, y:0, rotation:0, duration:0.8 }));

  // copy email
  document.getElementById('copyEmail').addEventListener('click', async ()=> {
    await navigator.clipboard.writeText('rudra.email@example.com');
    const notice = document.createElement('div'); notice.textContent='Email copied ✓'; notice.style.cssText='position:fixed;right:18px;bottom:18px;background:linear-gradient(90deg,var(--accent-a),var(--accent-b));color:#001;padding:10px 14px;border-radius:10px;z-index:80';
    document.body.appendChild(notice); setTimeout(()=>notice.remove(),1600);
  });

  // contact form fallback - Formspree works if you replace action
  const form = document.getElementById('contactForm');
  form.addEventListener('submit', (ev)=>{
    // If Formspree configured, it will submit. Otherwise show demo modal
    if(form.action.includes('formspree.io/f/your-id')){
      ev.preventDefault();
      const tmp = document.createElement('div');
      tmp.innerHTML = `<div style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:90"><div style="background:#021018;color:#dffcff;padding:22px;border-radius:12px;max-width:90%"><strong>Demo mode</strong><p style="color:var(--muted)">Your form action is still the placeholder. Replace with your Formspree endpoint to receive messages.</p><button id="okBtn" style="margin-top:12px;padding:8px 12px;border-radius:8px">OK</button></div></div>`;
      document.body.appendChild(tmp);
      document.getElementById('okBtn').addEventListener('click', ()=> tmp.remove());
    }
  });

  // navigation links smooth scroll
  document.querySelectorAll('.nav a').forEach(a=>{
    a.addEventListener('click', (e)=>{
      const href = a.getAttribute('href');
      if(href && href.startsWith('#')){
        e.preventDefault();
        document.querySelector(href).scrollIntoView({behavior:'smooth', block:'start'});
      }
    });
  });

  // keyboard shortcut: press "r" to download resume
  window.addEventListener('keydown', (e)=>{ if(e.key==='r') document.getElementById('download-resume').click(); });

})(); // ui()

/* ---------------------------
  Improvements you can do next (drop-in)
  - Upload your real assets to /assets (photo1..4, resume.pdf, favicon)
  - Replace Formspree action: https://formspree.io/f/<your-id>
  - Add analytics script (GA/ Plausible) if desired
  - Add real 3D glTF model if you want a more detailed satellite (I can embed for you)
  --------------------------- */

</script>
</body>
</html>
