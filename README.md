<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Rudi — The Technologist</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;900&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #fff;
      --text: #111;
      --accent: #000;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    body {
      background: var(--bg);
      color: var(--text);
      overflow-x: hidden;
    }

    header {
      text-align: center;
      padding: 6rem 2rem 3rem;
    }

    header h1 {
      font-size: 4rem;
      font-weight: 900;
      margin-bottom: 1rem;
      letter-spacing: -2px;
    }

    header p {
      font-size: 1.4rem;
      color: #555;
      max-width: 700px;
      margin: 0 auto;
      line-height: 1.6;
    }

    .hero-img {
      width: 220px;
      margin: 3rem auto;
      display: block;
      filter: drop-shadow(0 10px 40px rgba(0,0,0,0.2));
      transition: transform 0.6s ease;
    }

    .hero-img:hover {
      transform: scale(1.05) rotate(-2deg);
    }

    .section {
      max-width: 1000px;
      margin: 6rem auto;
      padding: 2rem;
      border-radius: 28px;
      background: rgba(255,255,255,0.6);
      box-shadow: 0 10px 40px rgba(0,0,0,0.08);
      transition: transform 0.6s ease, opacity 0.8s ease;
      opacity: 0;
      transform: translateY(60px);
    }

    .section h2 {
      font-size: 2.5rem;
      margin-bottom: 1rem;
      font-weight: 700;
      letter-spacing: -1px;
    }

    .section p {
      font-size: 1.2rem;
      line-height: 1.8;
      color: #333;
    }

    footer {
      text-align: center;
      padding: 4rem 2rem;
      font-size: 0.9rem;
      color: #666;
    }

    @media (max-width: 600px) {
      header h1 { font-size: 2.6rem; }
      header p, .section p { font-size: 1rem; }
      .section { margin: 3rem 1rem; }
    }
  </style>
</head>
<body>

  <header>
    <h1>Rudi</h1>
    <p>The digital samurai. Forged in Raipur. Sharpened in Varanasi. Unleashed at SRM. This is not a portfolio. This is a legacy in motion.</p>
    <img src="assets/rudi_avatar.svg" alt="Rudi Avatar" class="hero-img" />
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
    <p>Rudi began his academic life in Nalasopara, Mumbai. After a family conflict, he moved to Varanasi. UKG was repeated. From 3rd to 9th grade, he was a quiet struggler—barely passing, often overlooked. But the fire was building.</p>
  </div>

  <div class="section">
    <h2>Turning Point</h2>
    <p>In 10th grade, Rudi began Taekwondo and scored 72.4%. In 11th, he chose PCM—despite disbelief from others—and scored 65%. In 12th, he exploded: 14 medals, a black belt, and a dream to become school captain. He was denied the title, given Sports Captain instead.</p>
  </div>

  <div class="section">
    <h2>Legacy Projects</h2>
    <p>Rudi led a team of juniors and seniors to build a working model of the GSLV Mk III. The project made the newspaper. The school stayed silent. He anchored events, met the CM, performed Taekwondo demos, and fought for a farewell. He gave everything.</p>
  </div>

  <div class="section">
    <h2>Final Blow</h2>
    <p>Despite his efforts, Rudi scored 70% in boards. Then came the sting: the principal called him in—not to praise, but to scold. She blamed him for a lost sash that wasn’t his responsibility. She hadn’t chosen him. But he had chosen himself.</p>
  </div>

  <footer>
    &copy; 2025 Rudi. Inspired by Apple. Engineered to impress.
  </footer>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script>
    gsap.utils.toArray(".section").forEach((section, i) => {
      gsap.to(section, {
        scrollTrigger: {
          trigger: section,
          start: "top 80%",
          toggleActions: "play none none none"
        },
        opacity: 1,
        y: 0,
        duration: 1,
        delay: i * 0.2
      });
    });
  </script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
  <script>
    gsap.registerPlugin(ScrollTrigger);
  </script>

</body>
</html>
