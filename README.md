<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Rudra "Rudi" Pratap Singh — Mission Control</title>
  <meta name="description" content="Rudra Pratap Singh — personal portfolio: cinematic, high-performance, NASA-style interface." />

  <!-- Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;900&display=swap" rel="stylesheet">

  <!-- GSAP for smooth, powerful animations -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

  <style>
    :root{
      --bg:#02030a; --panel: rgba(255,255,255,0.04);
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

    /* full-screen stage */
    .stage{min-height:100vh; display:flex; align-items:center; justify-content:center; padding:60px 20px; position:relative;}

    /* star canvas behind everything */
    canvas#starfield{position:fixed; inset:0; z-index:0}

    /* central card */
    .card{
      position:relative; z-index:3; width:min(1100px,95%); border-radius:18px; padding:28px; background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border:1px solid var(--glass-border); box-shadow:0 10px 60px rgba(0,0,0,0.6), 0 0 40px rgba(0, 255, 224, 0.03);
      display:grid; grid-template-columns:360px 1fr; gap:28px; align-items:center; transform-style:preserve-3d; overflow:hidden;
    }

    /* neon ring around photo */
    .avatar-wrap{position:relative; width:320px; height:320px; display:flex; align-items:center; justify-content:center;}
    .avatar{
      width:260px; height:260px; border-radius:50%; overflow:hidden; border:2px solid rgba(255,255,255,0.06); display:block; object-fit:cover; background:linear-gradient(180deg,#0f2533,#07121a);
      box-shadow:0 6px 40px rgba(0,0,0,0.6), 0 0 30px rgba(0,255,224,0.06);
      transform:translateZ(40px);
    }
    .avatar-ring{
      position:absolute; width:340px; height:340px; border-radius:50%; top:50%; left:50%; transform:translate(-50%,-50%); pointer-events:none; mix-blend-mode:screen;
      background:conic-gradient(from 0deg, rgba(0,255,224,0.15), rgba(155,0,255,0.15)); filter:blur(12px);
      animation:spin 12s linear infinite; opacity:0.9;
    }
    @keyframes spin{to{transform:translate(-50%,-50%) rotate(360deg)}}

    /* quick personal info */
    .meta{padding:8px 0 0}
    h1.title{font-size:36px; letter-spacing:1px; margin:0 0 6px; font-weight:900; background:var(--accent); -webkit-background-clip:text; -webkit-text-fill-color:transparent; text-shadow:0 0 20px rgba(0,255,224,0.12)}
    .sub{color:#9fbfcf; margin-bottom:12px; font-weight:600}

    .stats{display:flex; gap:12px; flex-wrap:wrap; margin:14px 0}
    .stat{background:rgba(255,255,255,0.02); border:1px solid rgba(255,255,255,0.03); padding:10px 12px; border-radius:10px; font-weight:600; color:#dffcff}

    /* mission control buttons */
    .controls{display:flex; gap:12px; margin-top:18px}
    .btn{padding:10px 14px; border-radius:10px; border:1px solid rgba(255,255,255,0.06); background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); cursor:pointer; font-weight:700}
    .btn.primary{background:linear-gradient(90deg,var(--neon-a),var(--neon-b)); color:#001; box-shadow:0 6px 30px rgba(155,0,255,0.12); border:none}

    /* right column: mission bio + timeline */
    .bio{position:relative; padding:6px 6px 18px; transform:translateZ(20px)}
    .bio p{color:#d7f6f0; line-height:1.6}

    /* timeline */
    .timeline{margin-top:18px; display:flex; flex-direction:column; gap:12px}
    .event{display:flex; gap:16px; align-items:flex-start}
    .dot{width:12px; height:12px; border-radius:50%; background:linear-gradient(90deg,var(--neon-a),var(--neon-b)); box-shadow:0 0 18px rgba(0,255,224,0.2)}
    .event .ev-title{font-weight:800}
    .event .ev-sub{color:#9fbfcf; font-size:14px}

    /* floating telemetry panel */
    .telemetry{position:absolute; right:18px; top:18px; padding:10px 12px; border-radius:12px; background:rgba(0,0,0,0.4); border:1px solid rgba(255,255,255,0.03); backdrop-filter: blur(6px); font-size:13px}

    /* bottom full-width orbital section */
    .orbital{margin-top:40px; grid-column:1/-1; display:flex; gap:18px; align-items:center}
    .orbit-canvas{flex:1; min-height:180px; border-radius:12px; overflow:hidden}
    .skills{min-width:240px; padding:14px; border-radius:12px; background:linear-gradient(180deg, rgba(255,255,255,0.015), rgba(255,255,255,0.01)); border:1px solid rgba(255,255,255,0.03)}
    .skill-chip{display:inline-block; margin:6px 6px 0 0; padding:8px 10px; border-radius:9px; background:rgba(255,255,255,0.02); font-weight:700}

    /* small responsive tweaks */
    @media (max-width:980px){
      .card{grid-template-columns:1fr; padding:20px}
      .avatar-wrap{display:flex; justify-content:center}
      .telemetry{position:static; margin-top:10px}
      .orbital{flex-direction:column}
    }

    /* subtle footer */
    .credit{position:fixed; left:18px; bottom:18px; font-size:12px; color:#8fb7b0; z-index:6}

    /* pointer-following ripple */
    .cursor-ring{position:fixed; width:80px; height:80px; border-radius:50%; border:2px solid rgba(0,255,224,0.18); pointer-events:none; transform:translate(-50%,-50%) scale(0.8); mix-blend-mode:screen; transition:transform 0.12s linear; z-index:9}

  </style>
</head>
<body>

  <canvas id="starfield"></canvas>

  <div class="stage">
    <div class="card" id="card">

      <!-- LEFT: PHOTO + RING -->
      <div class="avatar-wrap">
        <!-- Avatar: REPLACE 'assets/your-photo.jpg' with your photo file (recommended 800x800, square) -->
        <img id="avatar" class="avatar" src="assets/your-photo.jpg" alt="Rudra Pratap Singh — Portrait">
        <div class="avatar-ring" aria-hidden="true"></div>
      </div>

      <!-- RIGHT: BIO, TIMELINE, CONTROLS -->
      <div class="bio">
        <div class="telemetry" id="telemetry">SYS: Nominal • Boot: 00:00:23 • Signal: Strong</div>

        <h1 class="title">Rudra "Rudi" Pratap Singh</h1>
        <div class="sub">Digital Samurai • Technologist • Taekwondo Black Belt • Mission: Iterate</div>

        <div class="meta">
          <div class="stats">
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
  </div>

  <div class="credit">Mission Control UI • Built for Rudi — tweak freely</div>
  <div class="cursor-ring" id="ring"></div>

  <!-- SCRIPT: INTERACTIONS + VISUALS -->
  <script>
    gsap.registerPlugin(ScrollTrigger);

    /* ---------- Starfield (canvas) ---------- */
    const starCanvas = document.getElementById('starfield');
    const sctx = starCanvas.getContext('2d');
    function resize(){ starCanvas.width = innerWidth; starCanvas.height = innerHeight; }
    resize(); window.addEventListener('resize', resize);

    const stars = Array.from({length:380}, ()=>({
      x: Math.random()*innerWidth,
      y: Math.random()*innerHeight,
      z: Math.random()*1.2 + 0.2,
      r: Math.random()*1.2 + 0.2
    }));

    function drawStars(){
      sctx.clearRect(0,0,starCanvas.width, starCanvas.height);
      for(const p of stars){
        p.y += 0.2*p.z;
        if(p.y>starCanvas.height){ p.y=0; p.x=Math.random()*starCanvas.width }
        const alpha = 0.3 + (p.z/1.4);
        sctx.beginPath(); sctx.fillStyle = 'rgba(255,255,255,'+alpha+')'; sctx.arc(p.x,p.y,p.r,0,Math.PI*2); sctx.fill();
      }
      requestAnimationFrame(drawStars);
    }
    drawStars();

    /* ---------- Avatar: floating tilt + mouse parallax ---------- */
    const card = document.getElementById('card');
    const avatar = document.getElementById('avatar');
    card.addEventListener('mousemove', (e)=>{
      const rect = card.getBoundingClientRect();
      const dx = (e.clientX - rect.left) / rect.width - 0.5;
      const dy = (e.clientY - rect.top) / rect.height - 0.5;
      gsap.to(card, {rotationY: dx*6, rotationX: -dy*6, transformOrigin:'center', duration:0.8, ease:'power3.out'});
      gsap.to(avatar, {x: dx*18, y: dy*18, rotation: dx*6, duration:0.9, ease:'power3.out'});
    });
    card.addEventListener('mouseleave', ()=>{ gsap.to([card,avatar], {rotationY:0, rotationX:0, x:0, y:0, rotation:0, duration:0.8, ease:'power3.out'}) });

    /* ---------- Orbit canvas: small particle orbit simulation ---------- */
    const orbitEl = document.getElementById('orbitCanvas');
    const ocan = document.createElement('canvas'); orbitEl.appendChild(ocan); const octx = ocan.getContext('2d');
    function orResize(){ ocan.width = orbitEl.clientWidth; ocan.height = orbitEl.clientHeight }
    orResize(); window.addEventListener('resize', orResize);
    const nodes = [];
    for(let i=0;i<18;i++) nodes.push({a:Math.random()*Math.PI*2, r: 40 + Math.random()*80, speed: (0.002+Math.random()*0.01), size:2+Math.random()*6});
    function drawOrbit(){
      octx.clearRect(0,0,ocan.width,ocan.height);
      const cx = ocan.width/2, cy = ocan.height/2;
      // central glow
      const g = octx.createRadialGradient(cx,cy,0,cx,cy,Math.max(ocan.width,ocan.height));
      g.addColorStop(0,'rgba(0,255,224,0.06)'); g.addColorStop(1,'rgba(0,0,0,0)');
      octx.fillStyle = g; octx.fillRect(0,0,ocan.width,ocan.height);
      // draw nodes
      for(const n of nodes){
        n.a += n.speed;
        const x = cx + Math.cos(n.a) * n.r * (1 + Math.sin(n.a*0.3)*0.18);
        const y = cy + Math.sin(n.a) * n.r * (1 + Math.cos(n.a*0.7)*0.12);
        octx.beginPath(); octx.fillStyle = 'rgba(255,255,255,0.9)'; octx.arc(x,y,n.size,0,Math.PI*2); octx.fill();
      }
      requestAnimationFrame(drawOrbit);
    }
    drawOrbit();

    /* ---------- Cursor ring ---------- */
    const ring = document.getElementById('ring');
    window.addEventListener('mousemove', (e)=>{ ring.style.left = e.clientX + 'px'; ring.style.top = e.clientY + 'px'; ring.style.transform = 'translate(-50%,-50%) scale(1)'; });
    window.addEventListener('mousedown', ()=>{ ring.style.transform = 'translate(-50%,-50%) scale(0.6)'; setTimeout(()=>ring.style.transform='translate(-50%,-50%) scale(1)',120) });

    /* ---------- Simple entrance animations with GSAP ---------- */
    gsap.from('.card',{duration:1.2, y:40, opacity:0, ease:'power4.out'});
    gsap.from('.title',{duration:1.2, y:6, opacity:0, delay:0.15, ease:'power3.out'});
    gsap.from('.stat',{duration:0.9, stagger:0.05, opacity:0, y:8});
    gsap.from('.event',{duration:0.9, stagger:0.08, opacity:0, y:18});

    /* ---------- Buttons ---------- */
    document.getElementById('contactBtn').addEventListener('click', ()=>{
      navigator.clipboard && navigator.clipboard.writeText('rudra.email@example.com');
      const b = document.getElementById('contactBtn'); b.innerText = 'Email Copied ✓'; setTimeout(()=>b.innerText='Contact',2000);
    });
    document.getElementById('missionBtn').addEventListener('click', ()=>{
      // open fake resume (replace with your real url) -- for demo we show a modal-like brief
      const modal = document.createElement('div'); modal.style.cssText='position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.6);z-index:9999';
      const box = document.createElement('div'); box.style.cssText='width:min(900px,95%);background:#021018;padding:22px;border-radius:12px;border:1px solid rgba(255,255,255,0.04);';
      box.innerHTML = '<h3 style="margin:0 0 8px">Launch Sequence Initiated</h3><p style="margin:0 0 12px;color:#9fbfcf">This demo opens a placeholder for your resume/portfolio PDF. Replace this with the real file link.</p><div style="display:flex;gap:8px"><a href="#" style="padding:8px 12px;background:linear-gradient(90deg,#00ffe0,#9b00ff);color:#001;border-radius:8px;text-decoration:none;font-weight:800">Open Resume</a><button id="closeModal" style="padding:8px 12px;border-radius:8px">Close</button></div>';
      modal.appendChild(box); document.body.appendChild(modal);
      document.getElementById('closeModal').addEventListener('click', ()=>modal.remove());
    });

    /* ---------- Replace telemetry live values (demo) ---------- */
    setInterval(()=>{
      document.getElementById('telemetry').innerText = `SYS: Nominal • Boot: ${Math.floor(Math.random()*60)}:${Math.floor(Math.random()*60).toString().padStart(2,'0')} • Signal: Strong`;
    }, 3000);

    /* ---------- Avatar fallback: if user doesn't replace image, use generated gradient avatar ---------- */
    avatar.addEventListener('error', ()=>{
      avatar.src = 'data:image/svg+xml;utf8,' + encodeURIComponent(`<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" width="800" height="800"><defs><linearGradient id="g" x1="0" x2="1"><stop offset="0" stop-color="#00ffe0"/><stop offset="1" stop-color="#9b00ff"/></linearGradient></defs><rect width="100%" height="100%" fill="#021018"/><circle cx="400" cy="320" r="200" fill="url(#g)" opacity="0.18"/><text x="50%" y="78%" text-anchor="middle" font-family="Inter, Arial" font-weight="700" font-size="64" fill="#bffcf3">RUDI</text></svg>`);
    });

    /* ---------- Accessibility hint: allow keyboard to trigger contact ---------- */
    document.getElementById('contactBtn').addEventListener('keyup', (e)=>{ if(e.key==='Enter') document.getElementById('contactBtn').click(); });

  </script>

  <!-- NOTES: 
     - Replace 'assets/your-photo.jpg' with your own portrait (square image recommended, e.g. 1200x1200). 
     - To host this: drop the file on any static host (GitHub Pages, Netlify, Vercel). 
     - If you want me to embed your actual photo and produce a trimmed, optimized copy, upload the image here and I will generate a web-optimized version. 
  -->

</body>
</html>
