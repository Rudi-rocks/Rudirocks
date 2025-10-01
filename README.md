<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Rudra "Rudi" Pratap Singh — Designed</title>
  <meta name="description" content="Rudra Pratap Singh — Clean, polished portfolio inspired by Apple's product pages." />

  <!-- Fonts: system-first for Apple-like feel -->
  <style>
    :root{
      --bg:#f5f6f7;            /* light neutral */
      --card:#ffffff;          /* card white */
      --muted:#6b7280;         /* subtle gray */
      --accent:#0a84ff;        /* blue accent (apple-ish) */
      --glass: rgba(255,255,255,0.6);
      --shadow: rgba(2,6,23,0.08);
      --radius: 14px;
      --max-w:1200px;
      --ease: cubic-bezier(.2,.9,.3,1);
      color-scheme: light;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, system-ui; background:var(--bg); color:#0b1220; -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
      -webkit-tap-highlight-color: transparent;
    }

    /* Layout */
    .wrap{max-width:var(--max-w); margin:48px auto; padding:48px 28px;}

    /* HEADER */
    header.top{display:flex; align-items:center; justify-content:space-between; gap:16px; margin-bottom:36px}
    .brand{display:flex; align-items:center; gap:12px}
    .logo{width:44px; height:44px; border-radius:10px; background:linear-gradient(135deg,#eef6ff,var(--glass)); display:flex; align-items:center; justify-content:center; box-shadow:0 6px 20px var(--shadow); font-weight:800; color:var(--accent)}
    .site-title{font-weight:700; letter-spacing:0.2px}
    nav.topnav{display:flex; gap:12px}
    nav.topnav a{font-weight:600;color:var(--muted); text-decoration:none; padding:8px 12px; border-radius:10px}
    nav.topnav a.cta{background:linear-gradient(90deg,var(--accent),#0066ff); color:white; box-shadow:0 8px 30px rgba(10,132,255,0.12)}

    /* HERO */
    .hero{display:grid; grid-template-columns:420px 1fr; gap:48px; align-items:center; background:linear-gradient(180deg, rgba(255,255,255,0.8), rgba(255,255,255,0.6)); padding:28px; border-radius:18px; box-shadow:0 12px 60px var(--shadow);}
    .portrait{width:380px; height:380px; border-radius:20px; overflow:hidden; background:linear-gradient(180deg,#fff,#f0f4ff); display:flex; align-items:center; justify-content:center; position:relative}
    .portrait img{width:100%; height:100%; object-fit:cover; display:block; transition:transform 0.9s var(--ease);}
    .portrait::after{content:''; position:absolute; inset:0; pointer-events:none; background:linear-gradient(180deg, rgba(255,255,255,0.0) 0%, rgba(255,255,255,0.25) 45%, rgba(255,255,255,0.0) 100%);}

    .hero h1{font-size:36px; margin:0 0 6px}
    .hero p.lead{margin:0 0 18px; color:var(--muted); font-size:15px; line-height:1.6}
    .hero .meta{display:flex; gap:10px; flex-wrap:wrap}
    .pill{padding:8px 12px; background:rgba(10,132,255,0.06); color:var(--accent); border-radius:999px; font-weight:700; border:1px solid rgba(10,132,255,0.08)}

    /* Features row - grid cards like Apple product specs */
    .features{display:flex; gap:16px; margin-top:22px}
    .feature{flex:1; background:var(--card); border-radius:12px; padding:16px; box-shadow:0 8px 30px var(--shadow); border:1px solid rgba(12,16,24,0.03)}
    .feature h4{margin:0 0 8px}
    .feature p{margin:0; color:var(--muted); font-size:14px}

    /* Projects gallery - cards */
    .projects{display:grid; grid-template-columns:repeat(3,1fr); gap:16px; margin-top:36px}
    .cardproj{background:var(--card); border-radius:12px; padding:14px; box-shadow:0 8px 30px var(--shadow); border:1px solid rgba(12,16,24,0.03); transition:transform 0.45s var(--ease), box-shadow 0.45s var(--ease)}
    .cardproj:hover{transform:translateY(-8px); box-shadow:0 18px 50px rgba(5,10,30,0.12)}
    .cardproj img{width:100%; height:140px; object-fit:cover; border-radius:8px; display:block}

    /* FOOTER + CONTACT */
    footer{margin-top:48px; color:var(--muted); font-size:14px; text-align:center}

    /* RESPONSIVE */
    @media (max-width:980px){
      .hero{grid-template-columns:1fr;}
      .projects{grid-template-columns:repeat(2,1fr)}
      .portrait{width:100%; height:360px}
    }
    @media (max-width:620px){
      .projects{grid-template-columns:1fr}
    }

    /* subtle entrance animations (CSS combined with JS for fluidity) */
    .fade-up{opacity:0; transform:translateY(14px); transition:opacity 0.8s var(--ease), transform 0.9s var(--ease)}
    .inview{opacity:1; transform:none}

    /* focus styles */
    a:focus, button:focus, input:focus{outline:3px solid rgba(10,132,255,0.14); outline-offset:3px}
  </style>
</head>
<body>

  <div class="wrap">
    <header class="top fade-up" id="header">
      <div class="brand">
        <div class="logo">R</div>
        <div>
          <div class="site-title">Rudra "Rudi" Pratap Singh</div>
          <div style="font-size:12px;color:var(--muted)">Technologist • Designer • Athlete</div>
        </div>
      </div>
      <nav class="topnav">
        <a href="#projects">Projects</a>
        <a href="#gallery">Gallery</a>
        <a href="#contact">Contact</a>
        <a class="cta" id="downloadBtn" href="assets/resume.pdf" download>Resume</a>
      </nav>
    </header>

    <section class="hero fade-up" id="home">
      <div class="portrait" id="portrait">
        <!-- Replace with your image at assets/your-photo.jpg -->
        <img id="heroImg" src="assets/your-photo.jpg" alt="Rudra Pratap Singh"/>
      </div>

      <div>
        <h1>Rudra "Rudi" Pratap Singh</h1>
        <p class="lead">Builder of things that move — from model rockets to embedded flight controllers. Taekwondo black belt. Leader. I make systems that work and stories that matter.</p>

        <div class="meta">
          <div class="pill">Raipur · Varanasi · SRM</div>
          <div class="pill">DOB: 23 Jun 2006</div>
          <div class="pill">Boards: 70%</div>
        </div>

        <div class="features">
          <div class="feature">
            <h4>Precision</h4>
            <p>Hardware-first mindset. Embedded systems and electronics with tight tolerances.</p>
          </div>
          <div class="feature">
            <h4>Leadership</h4>
            <p>Led cross-grade teams to deliver a working GSLV Mk III model and public demos.</p>
          </div>
          <div class="feature">
            <h4>Performance</h4>
            <p>14 medals in athletics and martial arts. Results-focused and relentless.</p>
          </div>
        </div>
      </div>
    </section>

    <section id="projects" class="fade-up">
      <h2 style="margin:28px 0 12px">Selected Projects</h2>
      <div class="projects">
        <div class="cardproj">
          <img src="assets/photo1.jpg" alt="GSLV model"/>
          <h3 style="margin:10px 0 6px">GSLV Mk III — Scale Model</h3>
          <p style="color:var(--muted)">Led a multidisciplinary student team to design, build and demonstrate a scale launch vehicle model.</p>
        </div>
        <div class="cardproj">
          <img src="assets/photo2.jpg" alt="Flight controller"/>
          <h3 style="margin:10px 0 6px">Embedded Flight Controller</h3>
          <p style="color:var(--muted)">Prototype flight controller for brushless motors and sensor fusion.</p>
        </div>
        <div class="cardproj">
          <img src="assets/photo3.jpg" alt="WebGL viz"/>
          <h3 style="margin:10px 0 6px">WebGL Telemetry Visualizer</h3>
          <p style="color:var(--muted)">3D visualization tools for trajectory and telemetry analysis.</p>
        </div>
      </div>
    </section>

    <section id="gallery" class="fade-up" style="margin-top:36px">
      <h2 style="margin-bottom:12px">Gallery & Press</h2>
      <div style="display:flex;gap:12px;flex-wrap:wrap">
        <img src="assets/photo1.jpg" alt="img1" style="width:220px;height:140px;object-fit:cover;border-radius:10px;box-shadow:0 8px 30px var(--shadow)"/>
        <img src="assets/photo2.jpg" alt="img2" style="width:220px;height:140px;object-fit:cover;border-radius:10px;box-shadow:0 8px 30px var(--shadow)"/>
        <img src="assets/photo3.jpg" alt="img3" style="width:220px;height:140px;object-fit:cover;border-radius:10px;box-shadow:0 8px 30px var(--shadow)"/>
        <img src="assets/photo4.jpg" alt="img4" style="width:220px;height:140px;object-fit:cover;border-radius:10px;box-shadow:0 8px 30px var(--shadow)"/>
      </div>
    </section>

    <section id="contact" class="fade-up" style="margin-top:36px">
      <h2>Contact</h2>
      <p style="color:var(--muted)">For opportunities, collaborations, or just to talk — drop a line.</p>
      <form id="contactForm" style="max-width:680px;margin-top:12px;display:grid;gap:10px">
        <input id="name" placeholder="Your name" style="padding:12px;border-radius:10px;border:1px solid rgba(0,0,0,0.06)" required />
        <input id="email" placeholder="Email" type="email" style="padding:12px;border-radius:10px;border:1px solid rgba(0,0,0,0.06)" required />
        <textarea id="message" placeholder="Message" rows="5" style="padding:12px;border-radius:10px;border:1px solid rgba(0,0,0,0.06)" required></textarea>
        <div style="display:flex;gap:10px">
          <button class="cta" type="submit" style="padding:12px 18px;border-radius:10px;background:linear-gradient(90deg,var(--accent),#0066ff);color:white;border:none;font-weight:700">Send</button>
          <button id="copyEmail" type="button" style="padding:12px 18px;border-radius:10px;border:1px solid rgba(0,0,0,0.06);background:white">Copy Email</button>
        </div>
      </form>
    </section>

    <footer>
      © 2025 Rudra "Rudi" Pratap Singh — Built with attention to detail.
    </footer>
  </div>

  <script>
    // Simple in-view observer to trigger CSS animations like Apple's subtle reveals
    const obs = new IntersectionObserver((entries)=>{
      entries.forEach(e=>{ if(e.isIntersecting) e.target.classList.add('inview'); });
    },{threshold:0.15});
    document.querySelectorAll('.fade-up').forEach(el=>obs.observe(el));

    // Portrait hover: zoom and subtle rotate
    const portrait = document.getElementById('portrait'); const heroImg = document.getElementById('heroImg');
    portrait.addEventListener('mousemove', (ev)=>{
      const r = portrait.getBoundingClientRect(); const cx = r.left + r.width/2; const cy = r.top + r.height/2; const dx = (ev.clientX - cx)/r.width; const dy = (ev.clientY - cy)/r.height; heroImg.style.transform = `translate(${dx*8}px,${dy*8}px) scale(1.04) rotate(${dx*3}deg)`;
    });
    portrait.addEventListener('mouseleave', ()=>{ heroImg.style.transform = 'none'; });

    // Replace image with placeholder if not found
    heroImg.addEventListener('error', ()=>{ heroImg.src = 'data:image/svg+xml;utf8,' + encodeURIComponent(`<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" width="1200" height="1200"><rect width="100%" height="100%" fill="#eef6ff"/><text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" font-family="system-ui" font-size="64" fill="#0a84ff">RUDI</text></svg>`); });

    // Contact form: integrate Formspree if you want. For now we'll copy email to clipboard and show a micro-notice
    document.getElementById('copyEmail').addEventListener('click', ()=>{ navigator.clipboard.writeText('rudra.email@example.com'); const t=document.createElement('div'); t.textContent='Email copied ✓'; t.style.cssText='position:fixed;right:20px;bottom:20px;background:#0a84ff;color:white;padding:10px 14px;border-radius:10px;box-shadow:0 8px 30px rgba(10,132,255,0.12);z-index:99'; document.body.appendChild(t); setTimeout(()=>t.remove(),2000); });

    // Submit handler — demo only: prevents default and shows a success micro-modal
    document.getElementById('contactForm').addEventListener('submit',(e)=>{
      e.preventDefault(); const n=document.getElementById('name').value || 'Friend'; const msg=document.createElement('div'); msg.innerHTML = `<div style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.4);z-index:9999"><div style='background:white;color:#001;padding:22px;border-radius:12px;max-width:90%;text-align:center'><strong>Thanks, ${n}!</strong><p style='color:#334155;margin-top:8px'>Message received (demo). Replace this handler with Formspree or Netlify Forms to receive real messages.</p><button id="ok" style='margin-top:14px;padding:8px 12px;border-radius:8px;background:linear-gradient(90deg,var(--accent),#0066ff);color:white;border:none'>OK</button></div></div>`; document.body.appendChild(msg); document.getElementById('ok').addEventListener('click', ()=>msg.remove()); });

    // Smooth scroll for nav
    document.querySelectorAll('nav.topnav a').forEach(a=>{ a.addEventListener('click',(ev)=>{ ev.preventDefault(); const t=document.querySelector(a.getAttribute('href')); if(t) t.scrollIntoView({behavior:'smooth',block:'start'}); }); });

    // Small preload animation: header slide
    window.addEventListener('load', ()=>{ document.getElementById('header').classList.add('inview'); });
  </script>
</body>
</html>
