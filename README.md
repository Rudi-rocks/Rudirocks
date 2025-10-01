<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Rudra "Rudi" Pratap Singh — Mission Control</title>
  <meta name="description" content="Rudra Pratap Singh — personal portfolio: cinematic, high-performance, NASA-style interface." />

  <!-- Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;900&display=swap" rel="stylesheet">

  <!-- GSAP + Three.js -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>

  <style>
    :root{
      --bg:#010214; --panel: rgba(255,255,255,0.03);
      --neon-a:#00ffe0; --neon-b:#9b00ff; --accent: linear-gradient(90deg,var(--neon-a),var(--neon-b));
      --glass-border: rgba(255,255,255,0.06);
      --mono: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; padding:0; font-family:var(--mono); background:radial-gradient(1200px 600px at 10% 10%, rgba(0,255,224,0.03), transparent), radial-gradient(900px 400px at 90% 90%, rgba(155,0,255,0.03), transparent), #000814; color:#e6f7ff; overflow-x:hidden;
      -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
    }

    /* NAV */
    header.site{position:fixed; inset:18px 18px auto 18px; z-index:60; display:flex; justify-content:space-between; align-items:center; gap:12px}
    .logo{font-weight:900; font-size:18px; letter-spacing:0.6px; background:var(--accent); -webkit-background-clip:text; -webkit-text-fill-color:transparent}
    nav{display:flex; gap:10px}
    nav a{padding:8px 12px; border-radius:8px; text-decoration:none; color:#bfeee9; font-weight:700; border:1px solid rgba(255,255,255,0.03); background:rgba(255,255,255,0.01)}

    /* full-screen stage */
    .stage{min-height:100vh; display:flex; align-items:center; justify-content:center; padding:120px 20px 80px; position:relative;}

    /* star canvas behind everything */
    canvas#starfield{position:fixed; inset:0; z-index:0}

    /* sections container */
    main{position:relative; z-index:3; width:100%;}
    .page{min-height:100vh; display:flex; align-items:center; justify-content:center; padding:80px 20px; scroll-snap-align:start}
    .pages{scroll-snap-type:y mandatory; overflow-y:auto; height:100vh}

    /* central card */
    .card{
      position:relative; z-index:3; width:min(1200px,95%); border-radius:18px; padding:28px; background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border:1px solid var(--glass-border); box-shadow:0 10px 80px rgba(0,0,0,0.6), 0 0 60px rgba(0, 255, 224, 0.03);
      display:grid; grid-template-columns:360px 1fr; gap:28px; align-items:center; transform-style:preserve-3d; overflow:hidden;
    }

    /* neon ring around photo */
    .avatar-wrap{position:relative; width:340px; height:340px; display:flex; align-items:center; justify-content:center}
    .avatar{width:280px; height:280px; border-radius:50%; overflow:hidden; border:2px solid rgba(255,255,255,0.06); display:block; object-fit:cover; background:linear-gradient(180deg,#0f2533,#07121a); box-shadow:0 6px 40px rgba(0,0,0,0.6), 0 0 40px rgba(0,255,224,0.06); transform:translateZ(40px)}
    .avatar-ring{position:absolute; width:360px; height:360px; border-radius:50%; top:50%; left:50%; transform:translate(-50%,-50%); pointer-events:none; mix-blend-mode:screen; background:conic-gradient(from 0deg, rgba(0,255,224,0.16), rgba(155,0,255,0.16)); filter:blur(12px); animation:spin 12s linear infinite;}
    @keyframes spin{to{transform:translate(-50%,-50%) rotate(360deg)}}

    /* right column: mission bio + timeline */
    .bio{position:relative; padding:6px 6px 18px; transform:translateZ(20px)}
    .bio p{color:#d7f6f0; line-height:1.6}

    .meta{display:flex; gap:12px; flex-wrap:wrap}
    .stat{background:rgba(255,255,255,0.02); border:1px solid rgba(255,255,255,0.03); padding:10px 12px; border-radius:10px; font-weight:600; color:#dffcff}

    .controls{display:flex; gap:12px; margin-top:18px}
    .btn{padding:10px 14px; border-radius:10px; border:1px solid rgba(255,255,255,0.06); background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); cursor:pointer; font-weight:700}
    .btn.primary{background:linear-gradient(90deg,var(--neon-a),var(--neon-b)); color:#001; box-shadow:0 6px 30px rgba(155,0,255,0.12); border:none}

    /* timeline */
    .timeline{margin-top:18px; display:flex; flex-direction:column; gap:12px}
    .event{display:flex; gap:16px; align-items:flex-start}
    .dot{width:12px; height:12px; border-radius:50%; background:linear-gradient(90deg,var(--neon-a),var(--neon-b)); box-shadow:0 0 18px rgba(0,255,224,0.2)}

    /* orbital section */
    .orbital{margin-top:28px; grid-column:1/-1; display:flex; gap:18px; align-items:center}
    .orbit-canvas{flex:1; min-height:260px; border-radius:12px; overflow:hidden}
    .skills{min-width:240px; padding:14px; border-radius:12px; background:linear-gradient(180deg, rgba(255,255,255,0.015), rgba(255,255,255,0.01)); border:1px solid rgba(255,255,255,0.03)}
    .skill-chip{display:inline-block; margin:6px 6px 0 0; padding:8px 10px; border-radius:9px; background:rgba(255,255,255,0.02); font-weight:700}

    /* other sections */
    .projects-grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:16px}
    .proj{padding:16px; border-radius:12px; background:rgba(255,255,255,0.02); border:1px solid rgba(255,255,255,0.03)}

    .gallery{display:flex; gap:10px; flex-wrap:wrap}
    .gallery img{width:160px; height:110px; object-fit:cover; border-radius:8px}

    .contact-form{display:grid; gap:10px; max-width:560px}
    input,textarea{padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:#dffcff}

    .credit{position:fixed; left:18px; bottom:18px; font-size:12px; color:#8fb7b0; z-index:6}
    .cursor-ring{position:fixed; width:80px; height:80px; border-radius:50%; border:2px solid rgba(0,255,224,0.18); pointer-events:none; transform:translate(-50%,-50%) scale(0.8); mix-blend-mode:screen; transition:transform 0.12s linear; z-index:9}

    @media (max-width:980px){
      .card{grid-template-columns:1fr; padding:20px}
      .avatar-wrap{display:flex; justify-content:center}
      .orbital{flex-direction:column}
      header.site{left:12px; right:12px}
    }
  </style>
</head>
<body>

  <canvas id="starfield"></canvas>
  <header class="site">
    <div class="logo">RUDI • MISSION CONTROL</div>
    <nav>
      <a href="#home">Home</a>
      <a href="#projects">Projects</a>
      <a href="#gallery">Gallery</a>
      <a href="#contact">Contact</a>
      <a id="downloadResume" href="assets/resume.pdf" download>Download Resume</a>
    </nav>
  </header>

  <div class="pages" id="pages">

    <!-- HOME -->
    <section class="page" id="home">
      <div class="card" id="card">

        <div class="avatar-wrap">
          <img id="avatar" class="avatar" src="assets/your-photo.jpg" alt="Rudra Pratap Singh — Portrait">
          <div class="avatar-ring" aria-hidden="true"></div>
        </div>

        <div class="bio">
          <div class="telemetry" id="telemetry">SYS: Nominal • Boot: 00:00:23 • Signal: Strong</div>

          <h1 class="title">Rudra "Rudi" Pratap Singh</h1>
          <div class="sub">Digital Samurai • Technologist • Taekwondo Black Belt • Mission: Iterate</div>

          <div class="meta">
            <div class="stat">DOB: 23 Jun 2006</div>
            <div class="stat">Raipur → Varanasi → SRM</div>
            <div class="stat">Age: 18</div>
            <div class="stat">Boards: 70%</div>
          </div>

          <div class="controls">
            <button class="btn" id="contactBtn">Contact</button>
            <button class="btn primary" id="missionBtn">Launch Resume</button>
            <button class="btn" id="toggleTheme">Night Vision</button>
          </div>

          <p style="margin-top:14px;">A legacy forged across cities, studios, and stadiums. From building a scale-model GSLV Mk III to leading teams and earning medals — this page is the telemetry of a life in motion.</p>

          <div class="timeline" aria-hidden="false">
            <div class="event"><div class="dot"></div><div><div class="ev-title">UKG — Reset</div><div class="ev-sub">Early childhood in Nalasopara, followed by move to Varanasi</div></div></div>
            <div class="event"><div class="dot"></div><div><div class="ev-title">10th Grade — Taekwondo</div><div class="ev-sub">Began training; board score 72.4%</div></div></div>
            <div class="event"><div class="dot"></div><div><div class="ev-title">12th Grade — Breakout</div><div class="ev-sub">14 medals, black belt, school leadership (Sports Captain)</div></div></div>
            <div class="event"><div class="dot"></div><div><div class="ev-title">Legacy Project — GSLV Mk III</div><div class="ev-sub">Led a student team; covered in press</div></div></div>
          </div>

          <div class="orbital">
            <div class="orbit-canvas" id="orbitCanvas"></div>
            <div class="skills">
              <div style="font-weight:900; margin-bottom:8px">Skills Orbit</div>
              <div class="skill-chip">C/C++</div>
              <div class="skill-chip">Embedded</div>
              <div class="skill-chip">Electronics</div>
              <div class="skill-chip">WebGL</div>
              <div class="skill-chip">Leadership</div>
              <div class="skill-chip">Taekwondo</div>
            </div>
          </div>

        </div>

      </div>
    </section>

    <!-- PROJECTS -->
    <section class="page" id="projects">
      <div style="width:min(1200px,95%); margin:0 auto;">
        <h2 style="background:var(--accent); -webkit-background-clip:text; -webkit-text-fill-color:transparent; font-size:32px; font-weight:900">Projects</h2>
        <p style="color:#9fbfcf; max-width:900px">Selection of legacy and technical projects. Click to expand.</p>
        <div class="projects-grid" style="margin-top:18px">
          <div class="proj"> <strong>GSLV Mk III Model</strong><p class="ev-sub">Lead — School tech project; printed prototype & press coverage.</p></div>
          <div class="proj"> <strong>Embedded Flight Controller</strong><p class="ev-sub">Prototype board controlling brushless motors and sensors.</p></div>
          <div class="proj"> <strong>WebGL Visualizer</strong><p class="ev-sub">3D visualizations for trajectories and telemetry.</p></div>
          <div class="proj"> <strong>Taekwondo Demo App</strong><p class="ev-sub">Skills tracker and timeline for training progress.</p></div>
        </div>
      </div>
    </section>

    <!-- GALLERY -->
    <section class="page" id="gallery">
      <div style="width:min(1200px,95%); margin:0 auto;">
        <h2 style="background:var(--accent); -webkit-background-clip:text; -webkit-text-fill-color:transparent; font-size:32px; font-weight:900">Gallery</h2>
        <p style="color:#9fbfcf; max-width:900px">Photos, event clippings, certificates.</p>
        <div class="gallery" style="margin-top:18px">
          <img src="assets/photo1.jpg" alt="gallery-1"/>
          <img src="assets/photo2.jpg" alt="gallery-2"/>
          <img src="assets/photo3.jpg" alt="gallery-3"/>
          <img src="assets/photo4.jpg" alt="gallery-4"/>
        </div>
      </div>
    </section>

    <!-- CONTACT -->
    <section class="page" id="contact">
      <div style="width:min(900px,95%); margin:0 auto;">
        <h2 style="background:var(--accent); -webkit-background-clip:text; -webkit-text-fill-color:transparent; font-size:32px; font-weight:900">Contact</h2>
        <p style="color:#9fbfcf; max-width:900px">Say hi, request the resume, or send project opportunities.</p>
        <form class="contact-form" action="mailto:rudra.email@example.com" method="post" enctype="text/plain">
          <input name="name" placeholder="Your name" />
          <input name="email" placeholder="Your email" />
          <textarea name="message" rows="6" placeholder="Message"></textarea>
          <div style="display:flex;gap:8px"><button class="btn primary" type="submit">Send</button><a class="btn" href="assets/resume.pdf" download>Download Resume</a></div>
        </form>
      </div>
    </section>

  </div>

  <div class="credit">Mission Control UI • Built for Rudi — tweak freely</div>
  <div class="cursor-ring" id="ring"></div>

  <script>
    gsap.registerPlugin(ScrollTrigger);
    /* Starfield */
    const starCanvas = document.getElementById('starfield'); const sctx = starCanvas.getContext('2d');
    function resize(){ starCanvas.width = innerWidth; starCanvas.height = innerHeight; }
    resize(); window.addEventListener('resize', resize);
    const stars = Array.from({length:420}, ()=>({x:Math.random()*innerWidth,y:Math.random()*innerHeight,z:Math.random()*1.6+0.2,r:Math.random()*1.3+0.2}));
    function drawStars(){ sctx.clearRect(0,0,starCanvas.width,starCanvas.height); for(const p of stars){ p.y+=0.25*p.z; if(p.y>starCanvas.height){p.y=0;p.x=Math.random()*starCanvas.width} const alpha = 0.25 + (p.z/1.6); sctx.beginPath(); sctx.fillStyle='rgba(255,255,255,'+alpha+')'; sctx.arc(p.x,p.y,p.r,0,Math.PI*2); sctx.fill(); } requestAnimationFrame(drawStars);} drawStars();

    /* Card parallax */
    const card = document.getElementById('card'); const avatar = document.getElementById('avatar');
    card.addEventListener('mousemove', (e)=>{ const rect = card.getBoundingClientRect(); const dx=(e.clientX-rect.left)/rect.width-0.5; const dy=(e.clientY-rect.top)/rect.height-0.5; gsap.to(card,{rotationY:dx*6,rotationX:-dy*6,duration:0.8,ease:'power3.out'}); gsap.to(avatar,{x:dx*18,y:dy*18,rotation:dx*6,duration:0.9,ease:'power3.out'}); });
    card.addEventListener('mouseleave', ()=>{ gsap.to([card,avatar],{rotationY:0,rotationX:0,x:0,y:0,rotation:0,duration:0.8,ease:'power3.out'})});

    /* Orbit canvas */
    const orbitEl = document.getElementById('orbitCanvas'); const ocan=document.createElement('canvas'); orbitEl.appendChild(ocan); const octx=ocan.getContext('2d'); function orResize(){ocan.width=orbitEl.clientWidth;ocan.height=orbitEl.clientHeight;} orResize(); window.addEventListener('resize', orResize);
    const nodes=[]; for(let i=0;i<20;i++) nodes.push({a:Math.random()*Math.PI*2,r:40+Math.random()*100,speed:(0.002+Math.random()*0.01),size:2+Math.random()*6}); function drawOrbit(){ octx.clearRect(0,0,ocan.width,ocan.height); const cx=ocan.width/2, cy=ocan.height/2; const g=octx.createRadialGradient(cx,cy,0,cx,cy,Math.max(ocan.width,ocan.height)); g.addColorStop(0,'rgba(0,255,224,0.06)'); g.addColorStop(1,'rgba(0,0,0,0)'); octx.fillStyle=g; octx.fillRect(0,0,ocan.width,ocan.height); for(const n of nodes){ n.a+=n.speed; const x=cx+Math.cos(n.a)*n.r*(1+Math.sin(n.a*0.3)*0.18); const y=cy+Math.sin(n.a)*n.r*(1+Math.cos(n.a*0.7)*0.12); octx.beginPath(); octx.fillStyle='rgba(255,255,255,0.9)'; octx.arc(x,y,n.size,0,Math.PI*2); octx.fill(); } requestAnimationFrame(drawOrbit);} drawOrbit();

    /* Cursor ring */
    const ring=document.getElementById('ring'); window.addEventListener('mousemove',(e)=>{ ring.style.left=e.clientX+'px'; ring.style.top=e.clientY+'px'; ring.style.transform='translate(-50%,-50%) scale(1)'; }); window.addEventListener('mousedown',()=>{ ring.style.transform='translate(-50%,-50%) scale(0.6)'; setTimeout(()=>ring.style.transform='translate(-50%,-50%) scale(1)',120) });

    gsap.from('.card',{duration:1.2,y:40,opacity:0,ease:'power4.out'}); gsap.from('.title',{duration:1.2,y:6,opacity:0,delay:0.15,ease:'power3.out'}); gsap.from('.stat',{duration:0.9,stagger:0.05,opacity:0,y:8}); gsap.from('.event',{duration:0.9,stagger:0.08,opacity:0,y:18});

    /* Buttons */
    document.getElementById('contactBtn').addEventListener('click', ()=>{ navigator.clipboard && navigator.clipboard.writeText('rudra.email@example.com'); const b=document.getElementById('contactBtn'); b.innerText='Email Copied ✓'; setTimeout(()=>b.innerText='Contact',2000); });

    document.getElementById('missionBtn').addEventListener('click', ()=>{ const modal=document.createElement('div'); modal.style.cssText='position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.6);z-index:9999'; const box=document.createElement('div'); box.style.cssText='width:min(900px,95%);background:#021018;padding:22px;border-radius:12px;border:1px solid rgba(255,255,255,0.04);'; box.innerHTML='<h3 style="margin:0 0 8px">Launch Sequence Initiated</h3><p style="margin:0 0 12px;color:#9fbfcf">Resume ready. Use the download button to save the PDF. You can replace the resume in assets/resume.pdf.</p><div style="display:flex;gap:8px"><a href="assets/resume.pdf" style="padding:8px 12px;background:linear-gradient(90deg,#00ffe0,#9b00ff);color:#001;border-radius:8px;text-decoration:none;font-weight:800" download>Open Resume</a><button id="closeModal" style="padding:8px 12px;border-radius:8px">Close</button></div>'; modal.appendChild(box); document.body.appendChild(modal); document.getElementById('closeModal').addEventListener('click', ()=>modal.remove()); });

    /* Telemetry demo */
    setInterval(()=>{ document.getElementById('telemetry').innerText = `SYS: Nominal • Boot: ${Math.floor(Math.random()*60)}:${Math.floor(Math.random()*60).toString().padStart(2,'0')} • Signal: Strong`; }, 3000);

    /* Smooth navigation */
    document.querySelectorAll('nav a').forEach(a=>{ a.addEventListener('click',(e)=>{ e.preventDefault(); document.querySelector(a.getAttribute('href')).scrollIntoView({behavior:'smooth'}); }); });

    /* Three.js micro-satellite in header (simple) */
    (function(){ const scene=new THREE.Scene(); const cam=new THREE.PerspectiveCamera(40,1,0.1,1000); const renderer=new THREE.WebGLRenderer({alpha:true,antialias:true}); const mount=document.createElement('div'); mount.style.cssText='position:absolute;right:8px;top:8px;width:220px;height:120px;pointer-events:none;z-index:5'; document.body.appendChild(mount); renderer.setSize(220,120); mount.appendChild(renderer.domElement); cam.position.z=3; const geo=new THREE.IcosahedronGeometry(0.6,1); const mat=new THREE.MeshStandardMaterial({emissive:0x00ffe0,emissiveIntensity:0.08,color:0x08121a,metalness:0.8,roughness:0.2}); const mesh=new THREE.Mesh(geo,mat); scene.add(mesh); const light=new THREE.DirectionalLight(0xffffff,0.8); light.position.set(5,5,5); scene.add(light); const amb=new THREE.AmbientLight(0x444444,0.8); scene.add(amb); function animate(){ requestAnimationFrame(animate); mesh.rotation.y+=0.008; mesh.rotation.x+=0.005; renderer.render(scene,cam);} animate(); })();

    /* Fallback avatar if missing */
    const av=document.getElementById('avatar'); av.addEventListener('error', ()=>{ av.src = 'data:image/svg+xml;utf8,' + encodeURIComponent('<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" width="800" height="800"><defs><linearGradient id="g" x1="0" x2="1"><stop offset="0" stop-color="#00ffe0"/><stop offset="1" stop-color="#9b00ff"/></linearGradient></defs><rect width="100%" height="100%" fill="#021018"/><circle cx="400" cy="320" r="200" fill="url(#g)" opacity="0.18"/><text x="50%" y="78%" text-anchor="middle" font-family="Inter, Arial" font-weight="700" font-size="64" fill="#bffcf3">RUDI</text></svg>'); });

  </script>

  <!-- NOTES: Replace assets/* with your files. To embed your photo and resume, upload them to the 'assets' folder on your host (photo: assets/your-photo.jpg, resume: assets/resume.pdf, gallery images: assets/photo1.jpg ...). -->
</body>
</html>
