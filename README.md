<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Rudi — The Technologist</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
  <style>
    :root {
      --accent: #00ffe0;
      --accent2: #ff00c8;
      --bg: #050505;
      --glass: rgba(255, 255, 255, 0.05);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    body {
      background: radial-gradient(circle at top, var(--bg), #000);
      color: #fff;
      overflow-x: hidden;
    }

    /* === STARFIELD BACKGROUND === */
    canvas#stars {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
    }

    header {
      text-align: center;
      padding: 8rem 2rem 4rem;
      perspective: 1000px;
    }

    header h1 {
      font-size: 5rem;
      font-weight: 900;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-shadow: 0 0 20px var(--accent), 0 0 40px var(--accent2);
      transform-style: preserve-3d;
    }

    header p {
      font-size: 1.5rem;
      color: #aaa;
      max-width: 700px;
      margin: 1.5rem auto 0;
    }

    /* === SECTIONS === */
    .section {
      max-width: 900px;
      margin: 6rem auto;
      padding: 2.5rem;
      background: var(--glass);
      border-radius: 20px;
      backdrop-filter: blur(12px);
      border: 1px solid rgba(0,255,255,0.2);
      box-shadow: 0 0 50px rgba(0, 255, 200, 0.1);
      opacity: 0;
      transform: translateY(80px) scale(0.95);
      transition: transform 0.3s ease;
    }

    .section:hover {
      transform: translateY(-10px) scale(1.02);
      box-shadow: 0 0 60px rgba(0, 255, 200, 0.3);
    }

    .section h2 {
      font-size: 2.2rem;
      margin-bottom: 1rem;
      color: var(--accent);
      text-shadow: 0 0 15px var(--accent);
      letter-spacing: 1px;
    }

    .section p {
      font-size: 1.2rem;
      line-height: 1.9;
      color: #ddd;
    }

    /* === TIMELINE EFFECT === */
    .section::before {
      content: "";
      position: absolute;
      width: 4px;
      height: 100%;
      background: linear-gradient(to bottom, var(--accent), var(--accent2));
      left: -40px;
      top: 0;
      border-radius: 10px;
    }

    footer {
      text-align: center;
      margin: 5rem 2rem;
      font-size: 0.9rem;
      color: #888;
    }

    @media (max-width: 600px) {
      header h1 {
        font-size: 3rem;
      }
      header p, .section p {
        font-size: 1rem;
      }
      .section {
        margin: 2rem 1rem;
      }
    }
  </style>
</head>
<body>

  <canvas id="stars"></canvas>

  <header>
    <h1 id="glitch-text">Rudi</h1>
    <p id="typing-text"></p>
  </header>

  <div class="section">
    <h2>Vital Stats</h2>
    <p>
      Name: <strong>Rudra Pratap Singh</strong><br/>
      Age: <strong>18</strong><br/>
      Height: <strong>165 cm</strong> | Weight: <strong>66 kg</strong><br/>
      Date of Birth: <strong>23rd June 2006</strong><br/>
      Birthplace: <strong>Raipur, Chhattisgarh</strong>
    </p>
  </div>

  <div class="section">
    <h2>Early Journey</h2>
    <p>
      Rudi began his academic life in Nalasopara, Mumbai. After a family conflict, he moved to Varanasi. UKG was repeated. From 3rd to 9th grade, he was a quiet struggler—barely passing, often overlooked. But the fire was building.
    </p>
  </div>

  <div class="section">
    <h2>Turning Point</h2>
    <p>
      In 10th grade, Rudi began Taekwondo and scored 72.4%. In 11th, he chose PCM—despite disbelief from others—and scored 65%. In 12th, he exploded: 14 medals, a black belt, and a dream to become school captain. He was denied the title, given Sports Captain instead.
    </p>
  </div>

  <div class="section">
    <h2>Legacy Projects</h2>
    <p>
      Rudi led a team of juniors and seniors to build a working model of the GSLV Mk III. The project made the newspaper. The school stayed silent. He anchored events, met the CM, performed Taekwondo demos, and fought for a farewell. He gave everything.
    </p>
  </div>

  <div class="section">
    <h2>Final Blow</h2>
    <p>
      Despite his efforts, Rudi scored 70% in boards. Then came the sting: the principal called him in—not to praise, but to scold. She blamed him for a lost sash that wasn’t his responsibility. She hadn’t chosen him. But he had chosen himself.
    </p>
  </div>

  <footer>
    &copy; 2025 Rudi. Engineered to impress. Designed to endure.
  </footer>

  <!-- GSAP -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
  
  <script>
    gsap.registerPlugin(ScrollTrigger);

    // Animate sections on scroll
    gsap.utils.toArray(".section").forEach((section, i) => {
      gsap.to(section, {
        scrollTrigger: {
          trigger: section,
          start: "top 85%",
          toggleActions: "play none none none"
        },
        opacity: 1,
        y: 0,
        scale: 1,
        duration: 1.2,
        ease: "power3.out",
        delay: i * 0.15
      });
    });

    // Typing Effect
    const text = "The digital samurai. Forged in Raipur. Sharpened in Varanasi. Unleashed at SRM. This is not a portfolio. This is a legacy in motion.";
    let index = 0;
    function type() {
      if (index < text.length) {
        document.getElementById("typing-text").innerHTML += text.charAt(index);
        index++;
        setTimeout(type, 40);
      }
    }
    type();

    // Starfield background
    const canvas = document.getElementById("stars");
    const ctx = canvas.getContext("2d");
    let stars = [];
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    for (let i = 0; i < 300; i++) {
      stars.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        radius: Math.random() * 1.2,
        speed: Math.random() * 0.3
      });
    }

    function drawStars() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "#fff";
      stars.forEach(star => {
        ctx.beginPath();
        ctx.arc(star.x, star.y, star.radius, 0, 2 * Math.PI);
        ctx.fill();
      });
    }

    function moveStars() {
      stars.forEach(star => {
        star.y += star.speed;
        if (star.y > canvas.height) {
          star.y = 0;
          star.x = Math.random() * canvas.width;
        }
      });
    }

    function animateStars() {
      drawStars();
      moveStars();
      requestAnimationFrame(animateStars);
    }
    animateStars();

    window.addEventListener("resize", () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });
  </script>
</body>
</html>
