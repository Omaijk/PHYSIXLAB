<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PHYSIXLAB | Physics Made Interactive</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">

<style>
:root{
  --glass:rgba(255,255,255,0.08);
}

*{
  box-sizing:border-box;
  font-family:'Poppins',sans-serif;
  scroll-behavior:smooth;
}

body{
  margin:0;
  color:#fff;
  background:#050816;
  overflow-x:hidden;
  opacity:0;
  transition:opacity .6s ease;
}

body.loaded{opacity:1}

#liquidCanvas{
  position:fixed;
  inset:0;
  z-index:-5;
  pointer-events:none;
}

#particleCanvas{
  position:fixed;
  inset:0;
  z-index:-2;
  pointer-events:none;
}

.loader{
  position:fixed;
  inset:0;
  background:#050816;
  display:flex;
  justify-content:center;
  align-items:center;
  z-index:9999;
  transition:opacity .6s ease, visibility .6s ease;
}

.loader.hidden{
  opacity:0;
  visibility:hidden;
}

.loader-content{
  text-align:center;
}

.loader-content h1{
  margin:20px 0 10px;
  font-size:2rem;
  letter-spacing:2px;
  background:linear-gradient(90deg,#ff4ecd,#4facfe);
  -webkit-background-clip:text;
  color:transparent;
}

.loader-content p{
  opacity:.7;
  font-size:.9rem;
}

.atom{
  width:80px;
  height:80px;
  border:3px solid transparent;
  border-top:3px solid #4facfe;
  border-right:3px solid #8f5cff;
  border-radius:50%;
  animation:spin 1.2s linear infinite;
  margin:auto;
}

@keyframes spin{
  from{transform:rotate(0deg)}
  to{transform:rotate(360deg)}
}

nav{
  position:sticky;
  top:0;
  z-index:100;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:16px 50px;
  background:rgba(0,0,0,.4);
  backdrop-filter:blur(18px);
  border-bottom:1px solid rgba(255,255,255,.06);
}

nav strong{
  font-size:1.2rem;
  letter-spacing:1px;
}

nav a{
  position:relative;
  color:white;
  text-decoration:none;
  margin:0 14px;
  font-weight:500;
}

nav a::after{
  content:"";
  position:absolute;
  left:0;
  bottom:-4px;
  width:0%;
  height:2px;
  background:linear-gradient(90deg,#ff4ecd,#4facfe);
  transition:.3s;
}

nav a:hover::after{
  width:100%;
}

.logo{
  display:flex;
  align-items:center;
  gap:10px;
}

.logo img{
  height:34px;
  width:auto;
}

header{
  min-height:90vh;
  display:grid;
  grid-template-columns:1.1fr 0.9fr;
  align-items:center;
  gap:60px;
  padding:60px 8%;
}

@media(max-width:900px){
  header{
    grid-template-columns:1fr;
    text-align:center;
  }
}

.hero-text,
.hero-visual{
  opacity:0;
  transform:translateY(40px);
  transition:.6s;
}

.hero-text.visible,
.hero-visual.visible{
  opacity:1;
  transform:translateY(0);
}

.hero-text h1{
  font-size:3.5rem;
  line-height:1.1;
  margin:0 0 20px;
  background:linear-gradient(90deg,#ff4ecd,#4facfe);
  -webkit-background-clip:text;
  color:transparent;
}

.hero-text p{
  max-width:520px;
  font-size:1.15rem;
  opacity:.9;
  margin-bottom:35px;
}

.btn{
  display:inline-block;
  padding:14px 30px;
  border-radius:18px;
  text-decoration:none;
  font-weight:600;
  transition:.3s;
  color:white;
  margin-right:14px;
}

.btn-primary{
  background:linear-gradient(90deg,#b3125f,#102a6b);
}

.btn-secondary{
  background:linear-gradient(90deg,#3d1c8a,#102a6b);
}

.btn:hover{
  transform:scale(1.05);
  box-shadow:0 0 20px rgba(79,172,254,.4);
}

.hero-visual{
  position:relative;
  transition:transform .1s linear;
}

.visual-card{
  background:var(--glass);
  border-radius:26px;
  padding:18px;
  backdrop-filter:blur(18px);
  box-shadow:0 0 40px rgba(0,0,0,.5);
}

.visual-card img{
  width:100%;
  border-radius:18px;
  display:block;
}

.orbit{
  position:absolute;
  top:-40px;
  right:-40px;
  width:140px;
  height:140px;
  border:1px dashed rgba(255,255,255,.2);
  border-radius:50%;
  animation:spin 18s linear infinite;
}

.orbit::after{
  content:'⚛';
  position:absolute;
  top:-14px;
  left:50%;
  transform:translateX(-50%);
  font-size:22px;
}

section{
  padding:80px 8%;
  text-align:center;
  position:relative;
}

section::before{
  content:"";
  position:absolute;
  top:0;
  left:50%;
  width:60%;
  height:2px;
  background:rgba(255,255,255,.06);
  transform:translateX(-50%);
}

.feature-grid{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:30px;
  margin-top:50px;
}

.card{
  background:var(--glass);
  border-radius:24px;
  padding:30px;
  backdrop-filter:blur(18px);
  transition:.4s;
  opacity:0;
  transform:translateY(40px);
  text-decoration:none;
  color:white;
}

.card.visible{
  opacity:1;
  transform:translateY(0);
}

.card:hover{
  transform:translateY(-10px);
  box-shadow:0 0 30px rgba(79,172,254,.3);
}

.card img{
  width:70px;
  height:70px;
  margin-bottom:15px;
}

footer{
  text-align:center;
  padding:40px;
  background:#03040d;
  opacity:.9;
}
</style>
</head>

<body>

<canvas id="liquidCanvas"></canvas>
<canvas id="particleCanvas"></canvas>

<div class="loader">
  <div class="loader-content">
    <div class="atom"></div>
    <h1>PHYSIXLAB</h1>
    <p>Launching materials...</p>
  </div>
</div>

<div id="loader-placeholder"></div>

<script>
fetch("loader.html")
  .then(res => res.text())
  .then(data => {
    document.getElementById("loader-placeholder").outerHTML = data;
  });
</script>

<nav>
  <div class="logo">
  <img src="laurensialogo.png" alt="Santa Laurensia Logo">
  <strong>PHYSIXLAB</strong>
  </div>
  <div>
    <a href="index.html">Home</a>
    <a href="kinematics.html">Kinematics</a>
    <a href="waves.html">Waves</a>
    <a href="staticelectricity.html">Static Electricity</a>
    <a href="magnetism.html">Magnetism</a>
    <a href="references.html">References</a>
    <a href="about.html">About</a>
  </div>
</nav>

<header>
  <div class="hero-text">
    <h1>Physics,<br>made interactive.</h1>
    <p>
      Interactive physics learning for Grade 7-9 topics.
      Explore concepts through presentations, simulations, videos, and exercises.
    </p>
    <a href="kinematics.html" class="btn btn-primary">Start Learning</a>
    <a href="https://dashboard.blooket.com/set/6997fd6d52d9b3ba7fb39298" class="btn btn-secondary">Quiz Mode</a>
  </div>

  <div class="hero-visual" id="parallaxLayer">
    <div class="orbit"></div>
    <div class="visual-card">
      <img src="physixlabvideo.gif" alt="Physics visualization">
    </div>
  </div>
</header>

<section>
  <h2>Explore Topics</h2>

  <div class="feature-grid">
    <a href="kinematics.html" class="card">
      <img src="kinematics-icon.png">
      <h3>Kinematics</h3>
      <p>Distance, displacement, speed, velocity, acceleration, distance-time graphs, speed-time graphs.</p>
    </a>

    <a href="waves.html" class="card">
      <img src="waves-icon.png">
      <h3>Waves</h3>
      <p>Concept and application of electromagnetic waves, wave behaviors (reflection, refraction).</p>
    </a>

    <a href="staticelectricity.html" class="card">
      <img src="staticelectricity-icon.png">
      <h3>Static Electricity</h3>
      <p>Electric charge, electrostatic phenomena, charging methods, Coulomb’s law, electric field and potential.</p>
    </a>

    <a href="magnetism.html" class="card">
      <img src="magnetism-icon.png">
      <h3>Magnetism</h3>
      <p>Magnets, magnetic poles, magnetic induction, methods of magnetization, magnetic field, electromagnetism.</p>
    </a>
  </div>
</section>

<footer>
  PHYSIXLAB — Educational Physics Platform
</footer>

<script>
window.addEventListener("load", () => {
  document.body.classList.add("loaded");
  setTimeout(()=>document.querySelector(".loader").classList.add("hidden"),800);
});

const canvas = document.getElementById("liquidCanvas");
const ctx = canvas.getContext("2d");

let w, h;
let mouseX = 0.5;
let mouseY = 0.5;

function resize(){
  w = canvas.width = window.innerWidth;
  h = canvas.height = window.innerHeight;
}
resize();
window.addEventListener("resize", resize);

window.addEventListener("mousemove", e=>{
  mouseX = e.clientX / w;
  mouseY = e.clientY / h;
});

class LavaBlob {
  constructor(){
    this.x = Math.random() * w;
    this.y = Math.random() * h;
    this.radius = 180 + Math.random() * 220;

    this.vx = (Math.random() - 0.5) * 0.2;
    this.vy = (Math.random() - 0.5) * 0.2;

    this.mass = this.radius * 0.02;
    this.temperature = Math.random(); // 0 (cool) to 1 (warm)

    this.color = this.randomColor();
  }

  randomColor(){
    const colors = [
  "#0f1e4d",
  "#16145c",
  "#1b1666",
  "#0a1a4a",
  "#1e2d70"
];
    return colors[Math.floor(Math.random()*colors.length)];
  }

  update(){

    this.temperature += (Math.random() - 0.5) * 0.005;
    this.temperature = Math.max(0, Math.min(1, this.temperature));

    const buoyancy = (this.temperature - 0.5) * 0.08;

    const gravity = 0.02;

    this.vy -= buoyancy;
    this.vy += gravity * 0.1;

    this.vx += Math.sin(Date.now() * 0.0003 + this.y * 0.01) * 0.01;

    const dx = mouseX * w - this.x;
    const dy = mouseY * h - this.y;
    this.vx += dx * 0.00001;
    this.vy += dy * 0.00001;

    this.vx *= 0.985;
    this.vy *= 0.985;

    this.x += this.vx;
    this.y += this.vy;

    if(this.y < -this.radius){
      this.temperature = 1;
      this.y = -this.radius;
      this.vy *= -0.3;
    }

    if(this.y > h + this.radius){
      this.temperature = 0;
      this.y = h + this.radius;
      this.vy *= -0.3;
    }

    if(this.x < -this.radius || this.x > w + this.radius){
      this.vx *= -0.5;
    }
  }

  draw(){
    const gradient = ctx.createRadialGradient(
      this.x,
      this.y,
      this.radius * 0.2,
      this.x,
      this.y,
      this.radius
    );

    gradient.addColorStop(0, this.color);
    gradient.addColorStop(0.5, this.color + "CC");
    gradient.addColorStop(1, "transparent");

    ctx.fillStyle = gradient;
    ctx.save();
    ctx.filter = "blur(40px)";
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}

const blobs = [];
for(let i = 0; i < 10; i++){
  blobs.push(new LavaBlob());
}

function animateLiquid(){

  ctx.clearRect(0,0,w,h);

  ctx.fillStyle = "#0a0f2f";
  ctx.fillRect(0,0,w,h);

  ctx.globalCompositeOperation = "screen";

  blobs.forEach(blob=>{
    blob.update();
    blob.draw();
  });

  ctx.globalCompositeOperation = "source-over";

  requestAnimationFrame(animateLiquid);
}

animateLiquid();

const pCanvas=document.getElementById("particleCanvas");
const pCtx=pCanvas.getContext("2d");
pCanvas.width=w;
pCanvas.height=h;

let particles=[];
for(let i=0;i<60;i++){
  particles.push({
    x:Math.random()*w,
    y:Math.random()*h,
    r:Math.random()*2+1,
    vx:(Math.random()-.5)*.3,
    vy:(Math.random()-.5)*.3
  });
}

function drawParticles(){
  pCtx.clearRect(0,0,w,h);
  pCtx.fillStyle="rgba(255,255,255,.2)";
  particles.forEach(p=>{
    p.x+=p.vx+(mouseX-.5)*0.3;
    p.y+=p.vy+(mouseY-.5)*0.3;
    if(p.x<0||p.x>w)p.vx*=-1;
    if(p.y<0||p.y>h)p.vy*=-1;
    pCtx.beginPath();
    pCtx.arc(p.x,p.y,p.r,0,Math.PI*2);
    pCtx.fill();
  });
  requestAnimationFrame(drawParticles);
}
drawParticles();

const parallax=document.getElementById("parallaxLayer");
window.addEventListener("mousemove",e=>{
  const x=(e.clientX-w/2)/40;
  const y=(e.clientY-h/2)/40;
  parallax.style.transform=`translate(${x}px,${y}px)`;
});

const observer=new IntersectionObserver(entries=>{
  entries.forEach(entry=>{
    if(entry.isIntersecting){
      entry.target.classList.add("visible");
    }
  });
},{threshold:.2});

document.querySelectorAll(".card,.hero-text,.hero-visual").forEach(el=>{
  observer.observe(el);
});
</script>

</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PHYSIXLAB | Kinematics</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">

<style>
:root{
  --pink:#ff4ecd;
  --blue:#4facfe;
  --purple:#8f5cff;
  --yellow:#ffe066;
  --glass:rgba(255,255,255,0.12);
}

*{
  box-sizing:border-box;
  font-family:'Poppins',sans-serif;
  scroll-behavior:smooth;
}

body{
  margin:0;
  background:linear-gradient(-45deg,#0b0d2a,#1b1f63,#1a1a4f,#11143b);
  background-size:400% 400%;
  animation:gradientMove 15s ease infinite;
  color:#fff;
  opacity:0;
  transition:opacity .6s ease;
}

@keyframes gradientMove{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}

body.loaded{opacity:1}

nav{
  position:sticky;
  top:0;
  z-index:100;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:16px 50px;
  background:rgba(0,0,0,.4);
  backdrop-filter:blur(18px);
  border-bottom:1px solid rgba(255,255,255,.08);
}

nav strong{
  font-size:1.2rem;
  letter-spacing:1px;
}

nav a{
  position:relative;
  color:white;
  text-decoration:none;
  margin:0 14px;
  font-weight:500;
  transition:.3s;
}

nav a::after{
  content:"";
  position:absolute;
  left:0;
  bottom:-4px;
  width:0%;
  height:2px;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  transition:.3s;
}

nav a:hover::after{
  width:100%;
}

.logo{
  display:flex;
  align-items:center;
  gap:10px;
}

.logo img{
  height:34px;
  width:auto;
}

header{
  padding:110px 8% 80px;
  text-align:center;
  position:relative;
}

header h1{
  font-size:3.2rem;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  -webkit-background-clip:text;
  color:transparent;
}

header p{
  max-width:750px;
  margin:auto;
  opacity:.9;
}

header::after{
  content:"";
  position:absolute;
  width:250px;
  height:250px;
  background:radial-gradient(circle,var(--blue),transparent);
  top:30%;
  left:50%;
  transform:translateX(-50%);
  filter:blur(90px);
  opacity:.25;
}

section{
  padding:80px 8%;
  position:relative;
}

section::before{
  content:"";
  position:absolute;
  top:0;
  left:50%;
  width:60%;
  height:2px;
  background:rgba(255,255,255,.08);
  transform:translateX(-50%);
}

h2{
  text-align:center;
  font-size:2.3rem;
}

.subtitle{
  text-align:center;
  opacity:.8;
  margin-bottom:50px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
  gap:32px;
}

.card{
  background:var(--glass);
  border-radius:24px;
  padding:28px;
  backdrop-filter:blur(18px);
  transition:.4s;
  opacity:0;
  transform:translateY(40px);
}

.card.visible{
  opacity:1;
  transform:translateY(0);
}

.card:hover{
  transform:translateY(-10px) scale(1.02);
  box-shadow:0 0 35px rgba(79,172,254,.4);
}

.card h3{
  margin-top:0;
  color:var(--yellow);
}

.formula{
  background:rgba(0,0,0,.4);
  padding:12px;
  border-radius:12px;
  font-family:monospace;
  margin:14px 0;
  text-align:center;
}

.fact{
  background:rgba(0,0,0,.3);
  padding:12px;
  border-left:4px solid var(--blue);
  border-radius:12px;
  margin-top:12px;
  font-size:.95rem;
}

.btn{
  display:inline-block;
  padding:14px 30px;
  border-radius:18px;
  text-decoration:none;
  font-weight:600;
  transition:.3s;
  color:white;
}

.btn-primary{
  background:linear-gradient(90deg,var(--pink),var(--blue));
}

.btn-secondary{
  background:linear-gradient(90deg,var(--purple),var(--blue));
}

.btn:hover{
  transform:scale(1.05);
  box-shadow:0 0 20px rgba(79,172,254,.6);
}

.video-grid img{
  width:100%;
  border-radius:18px;
  transition:.4s;
}

.video-grid img:hover{
  transform:scale(1.05);
  box-shadow:0 0 30px rgba(255,255,255,.3);
}

iframe{
  width:100%;
  height:500px;
  border:none;
  border-radius:20px;
  box-shadow:0 0 40px rgba(79,172,254,.3);
}

footer{
  text-align:center;
  padding:40px;
  background:#070816;
  opacity:.85;
  margin-top:60px;
}
</style>
</head>

<body>

<nav>
  <div class="logo">
  <img src="laurensialogo.png" alt="Santa Laurensia Logo">
  <strong>PHYSIXLAB</strong>
  </div>
  <div>
    <a href="index.html">Home</a>
    <a href="kinematics.html">Kinematics</a>
    <a href="waves.html">Waves</a>
    <a href="staticelectricity.html">Static Electricity</a>
    <a href="magnetism.html">Magnetism</a>
    <a href="references.html">References</a>
    <a href="about.html">About</a>
  </div>
</nav>

<header>
  <h1>Kinematics</h1>
  <p>
    Kinematics is the branch of classical mechanics that describes the motion of points, 
    objects and systems of groups of objects, without reference to the causes of motion. 
    To describe motion, kinematics studies the trajectories of points, lines and other geometric 
    objects, as well as their differential properties (such as velocity and acceleration).
  </p>
</header>

<section>
  <h2>Core Concepts</h2>
  <p class="subtitle">Understanding the main topics is key to mastering kinematics.</p>

  <div style="display:grid;grid-template-columns:2fr 1.1fr;gap:60px;align-items:start;">

    <div style="display:flex;flex-direction:column;gap:50px;height:520px;">

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:35px;">
        
        <div class="card">
          <h3>Distance & Displacement</h3>
          <p>Understand the difference between scalar and vector quantity 
            for <strong>Distance</strong> and <strong>Displacement</strong>.</p>
          <div class="fact">Distance is scalar. Displacement is vector.</div>
        </div>

        <div class="card">
          <h3>Speed</h3>
          <p>Understand the difference between scalar and vector quantity 
            and use the relationship between <strong>average speed</strong>, 
            <strong>distance moved</strong>, and <strong>time</strong> in the formula.</p>
          <div class="formula">Speed = Distance ÷ Time</div>
        </div>

      </div>

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:35px;">
        
        <div class="card">
          <h3>Acceleration</h3>
          <p>Use the relationship between <strong>acceleration</strong>, change in 
            <strong>velocity</strong> and <strong>time</strong> taken in the formula.</p>
          <div class="formula">Acceleration = ΔVelocity ÷ Time</div>
        </div>

        <div class="card">
          <h3>Distance-Time & Speed-Time Graphs</h3>
          <p>Investigate and interpret the data correctly in <strong>distance-time graphs</strong>.</p>
          <p>Determine the distance travelled from the area of a <strong>speed-time graph</strong>.</p>
        </div>

      </div>

    </div>

    <div class="card" style="padding:25px;">
      <h3 style="text-align:center;">Theory & Formula</h3>

      <embed 
        src="KINEMATICS%20-%20Theory%20and%20Formula.pdf"
        type="application/pdf"
        style="width:100%;height:480px;border:none;border-radius:18px;margin-top:15px;box-shadow:0 0 30px rgba(79,172,254,.3);">

      <div style="text-align:center;margin-top:18px;">
        <a href="KINEMATICS%20-%20Theory%20and%20Formula.pdf"
           target="_blank"
           class="btn btn-primary">
           Open Full PDF
        </a>
      </div>
    </div>

  </div>
</section>

<section>
  <h2>Interactive Simulation</h2>
  <p class="subtitle">Explore distance-time and velocity-time graphs visually</p>

  <iframe src="https://iwant2study.org/lookangejss/02_newtonianmechanics_2kinematics/ejss_model_Kinematicsfukwun/Kinematicsfukwun_Simulation.xhtml"></iframe>
</section>

<section>
  <h2>Learn & Practice</h2>
  <p class="subtitle">Study the material and test your understanding via Blooket</p>

  <div class="card" style="max-width:700px;margin:auto;text-align:center;">
    <embed src="KINEMATICS%20PHYSIXLAB.pdf" type="application/pdf" style="width:100%;height:400px;margin-bottom:20px;">

    <div style="display:flex;justify-content:center;gap:20px;flex-wrap:wrap;">
      <a href="KINEMATICS%20PHYSIXLAB.pdf" target="_blank" class="btn btn-primary">Open PDF</a>
      <a href="https://dashboard.blooket.com/set/6997fd6d52d9b3ba7fb39298" target="_blank" class="btn btn-secondary">Practice Quiz</a>
    </div>
  </div>
</section>

<section>
  <h2>Related Videos</h2>
  <p class="subtitle">Click to watch on YouTube</p>

  <div class="grid video-grid">
    <a href="https://www.youtube.com/watch?v=D0Hkd5TaaTQ">
      <img src="https://img.youtube.com/vi/D0Hkd5TaaTQ/0.jpg">
    </a>
    <a href="https://www.youtube.com/watch?v=NZvMXcIztuU" target="_blank">
      <img src="https://img.youtube.com/vi/NZvMXcIztuU/0.jpg">
    </a>
    <a href="https://www.youtube.com/watch?v=ysda01Y8PgE" target="_blank">
      <img src="https://img.youtube.com/vi/ysda01Y8PgE/0.jpg">
    </a>
    <a href="https://www.youtube.com/watch?v=RM02SnuJ0MY" target="_blank">
      <img src="https://img.youtube.com/vi/RM02SnuJ0MY/0.jpg">
    </a>
    <a href="https://www.youtube.com/watch?v=VJefeYJL3uE" target="_blank">
      <img src="https://img.youtube.com/vi/VJefeYJL3uE/0.jpg">
    </a>
  </div>
</section>

<footer>
  PHYSIXLAB — Educational Physics Platform
</footer>

<script>
window.addEventListener("load", () => {
  document.body.classList.add("loaded");
});

const observer = new IntersectionObserver(entries=>{
  entries.forEach(entry=>{
    if(entry.isIntersecting){
      entry.target.classList.add("visible");
    }
  });
},{threshold:0.2});

document.querySelectorAll(".card").forEach(card=>{
  observer.observe(card);
});
</script>

</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PHYSIXLAB | Waves</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">

<style>
:root{
  --pink:#ff4ecd;
  --blue:#4facfe;
  --purple:#8f5cff;
  --yellow:#ffe066;
  --glass:rgba(255,255,255,0.12);
}

*{
  box-sizing:border-box;
  font-family:'Poppins',sans-serif;
  scroll-behavior:smooth;
}

body{
  margin:0;
  background:linear-gradient(-45deg,#0b0d2a,#1b1f63,#1a1a4f,#11143b);
  background-size:400% 400%;
  animation:gradientMove 15s ease infinite;
  color:#fff;
  opacity:0;
  transition:opacity .6s ease;
}

@keyframes gradientMove{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}

body.loaded{opacity:1}

nav{
  position:sticky;
  top:0;
  z-index:100;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:16px 50px;
  background:rgba(0,0,0,.4);
  backdrop-filter:blur(18px);
  border-bottom:1px solid rgba(255,255,255,.08);
}

nav strong{
  font-size:1.2rem;
  letter-spacing:1px;
}

nav a{
  position:relative;
  color:white;
  text-decoration:none;
  margin:0 14px;
  font-weight:500;
  transition:.3s;
}

nav a::after{
  content:"";
  position:absolute;
  left:0;
  bottom:-4px;
  width:0%;
  height:2px;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  transition:.3s;
}

nav a:hover::after{
  width:100%;
}

header{
  padding:110px 8% 80px;
  text-align:center;
  position:relative;
}

header h1{
  font-size:3.2rem;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  -webkit-background-clip:text;
  color:transparent;
}

header p{
  max-width:750px;
  margin:auto;
  opacity:.9;
}

header::after{
  content:"";
  position:absolute;
  width:250px;
  height:250px;
  background:radial-gradient(circle,var(--blue),transparent);
  top:30%;
  left:50%;
  transform:translateX(-50%);
  filter:blur(90px);
  opacity:.25;
}

section{
  padding:80px 8%;
  position:relative;
}

section::before{
  content:"";
  position:absolute;
  top:0;
  left:50%;
  width:60%;
  height:2px;
  background:rgba(255,255,255,.08);
  transform:translateX(-50%);
}

h2{
  text-align:center;
  font-size:2.3rem;
}

.subtitle{
  text-align:center;
  opacity:.8;
  margin-bottom:50px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
  gap:32px;
}

.card{
  background:var(--glass);
  border-radius:24px;
  padding:28px;
  backdrop-filter:blur(18px);
  transition:.4s;
  opacity:0;
  transform:translateY(40px);
}

.card.visible{
  opacity:1;
  transform:translateY(0);
}

.card:hover{
  transform:translateY(-10px) scale(1.02);
  box-shadow:0 0 35px rgba(79,172,254,.4);
}

.card h3{
  margin-top:0;
  color:var(--yellow);
}

.formula{
  background:rgba(0,0,0,.4);
  padding:12px;
  border-radius:12px;
  font-family:monospace;
  margin:14px 0;
  text-align:center;
}

.fact{
  background:rgba(0,0,0,.3);
  padding:12px;
  border-left:4px solid var(--blue);
  border-radius:12px;
  margin-top:12px;
  font-size:.95rem;
}

.btn{
  display:inline-block;
  padding:14px 30px;
  border-radius:18px;
  text-decoration:none;
  font-weight:600;
  transition:.3s;
  color:white;
}

.btn-primary{
  background:linear-gradient(90deg,var(--pink),var(--blue));
}

.btn-secondary{
  background:linear-gradient(90deg,var(--purple),var(--blue));
}

.btn:hover{
  transform:scale(1.05);
  box-shadow:0 0 20px rgba(79,172,254,.6);
}

.video-grid img{
  width:100%;
  border-radius:18px;
  transition:.4s;
}

.video-grid img:hover{
  transform:scale(1.05);
  box-shadow:0 0 30px rgba(255,255,255,.3);
}

iframe{
  width:100%;
  height:500px;
  border:none;
  border-radius:20px;
  box-shadow:0 0 40px rgba(79,172,254,.3);
}

footer{
  text-align:center;
  padding:40px;
  background:#070816;
  opacity:.85;
  margin-top:60px;
}
</style>
</head>

<body>

<nav>
  <strong>PHYSIXLAB</strong>
  <div>
    <a href="index.html">Home</a>
    <a href="kinematics.html">Kinematics</a>
    <a href="waves.html">Waves</a>
    <a href="staticelectricity.html">Static Electricity</a>
    <a href="magnetism.html">Magnetism</a>
    <a href="references.html">References</a>
    <a href="about.html">About</a>
  </div>
</nav>

<header>
  <h1>Waves</h1>
  <p>
    A wave is a propagating disturbance that transfers energy from one location to another through a medium (or vacuum) without transporting matter. 
    As the energy moves, particles of the medium oscillate around a fixed equilibrium position, creating repeating, periodic patterns.
  </p>
</header>

<section>
  <h2>Core Concepts</h2>
  <p class="subtitle">Understanding the main topics is key to mastering wave behaviors.</p>

  <div style="display:grid;grid-template-columns:2fr 1.1fr;gap:60px;align-items:start;">

    <div style="display:flex;flex-direction:column;gap:50px;height:520px;">

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:35px;">
        
        <div class="card">
          <h3>Vibration</h3>
          <p>Calculate the period, frequency, or the amount of <strong>vibrations</strong>.
        </div>

        <div class="card">
          <h3>Wave Motion</h3>
          <p>Analyze experiment data, understand wave motion, calculate quantities related to waves, determine the type of wave based on the object/shape.</p>
        </div>

      </div>

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:35px;">
        
        <div class="card">
          <h3>Transverse and Longitudinal Waves</h3>
          <p>Understand the difference between <strong>transverse</strong> and  
            <strong>longitudinal</strong> waves and determine the wavelength of the transverse wave given.</p>
        </div>

        <div class="card">
          <h3>Mechanical and Electromagnetic Waves</h3>
          <p>Understand the difference between <strong>mechanical</strong> and
          <strong>electromagnetic</strong> wave.</p>
        </div>

      </div>

    </div>

    <div class="card" style="padding:25px;">
      <h3 style="text-align:center;">Theory & Formula</h3>

      <embed 
        src="WAVE FORMULA.pdf"
        type="application/pdf"
        style="width:100%;height:400px;border:none;border-radius:18px;margin-top:15px;box-shadow:0 0 30px rgba(79,172,254,.3);">

      <div style="text-align:center;margin-top:18px;">
        <a href="WAVE FORMULA.pdf"
           target="_blank"
           class="btn btn-primary">
           Open Full PDF
        </a>
      </div>
    </div>

  </div>
</section>

<section>
  <h2>Interactive Simulation</h2>
  <p class="subtitle">Explore the concept of waves visually</p>

  <iframe src="https://phet.colorado.edu/sims/html/wave-on-a-string/latest/wave-on-a-string_en.html"></iframe>
</section>

<section>
  <h2>Learn & Practice</h2>
  <p class="subtitle">Study the material and test your understanding via Blooket</p>

  <div class="card" style="max-width:700px;margin:auto;text-align:center;">
    <embed src="Wave.pdf" type="application/pdf" style="width:100%;height:400px;margin-bottom:20px;">

    <div style="display:flex;justify-content:center;gap:20px;flex-wrap:wrap;">
      <a href="Wave.pdf" target="_blank" class="btn btn-primary">Open PDF</a>
      <a href="https://dashboard.blooket.com/set/69a1a2647024c277731ebd8b" target="_blank" class="btn btn-secondary">Practice Quiz</a>
    </div>
  </div>
</section>

<section>
  <h2>Related Videos</h2>
  <p class="subtitle">Click to watch on YouTube</p>

  <div class="grid video-grid">
    <a href="https://www.youtube.com/watch?si=FcSCwRvAHd8VS9Ic&v=1DFAy8MXkMA">
      <img src="https://img.youtube.com/vi/FcSCwRvAHd8VS9Ic&v=1DFAy8MXkMA/0.jpg">
    </a>
    <a href="https://www.youtube.com/watch?v=zx0UIMA2-bA" target="_blank">
      <img src="https://img.youtube.com/vi/zx0UIMA2-bA/0.jpg">
  </div>
</section>

<footer>
  PHYSIXLAB — Educational Physics Platform
</footer>

<script>
window.addEventListener("load", () => {
  document.body.classList.add("loaded");
});

const observer = new IntersectionObserver(entries=>{
  entries.forEach(entry=>{
    if(entry.isIntersecting){
      entry.target.classList.add("visible");
    }
  });
},{threshold:0.2});

document.querySelectorAll(".card").forEach(card=>{
  observer.observe(card);
});
</script>

</body>
</html>2

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>PHYSIXLAB | About</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">

<style>
:root{
  --pink:#ff4ecd;
  --blue:#4facfe;
  --purple:#8f5cff;
  --yellow:#ffe066;
  --glass:rgba(255,255,255,0.12);
}

*{
  box-sizing:border-box;
  font-family:'Poppins',sans-serif;
  scroll-behavior:smooth;
}

body{
  margin:0;
  background:linear-gradient(-45deg,#0b0d2a,#1b1f63,#1a1a4f,#11143b);
  background-size:400% 400%;
  animation:gradientMove 15s ease infinite;
  color:#fff;
  opacity:0;
  transition:opacity .6s ease;
}

@keyframes gradientMove{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}

body.loaded{opacity:1}

nav{
  position:sticky;
  top:0;
  z-index:100;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:16px 50px;
  background:rgba(0,0,0,.4);
  backdrop-filter:blur(18px);
  border-bottom:1px solid rgba(255,255,255,.06);
}

nav strong{
  font-size:1.2rem;
  letter-spacing:1px;
}

nav a{
  position:relative;
  color:white;
  text-decoration:none;
  margin:0 14px;
  font-weight:500;
}

nav a::after{
  content:"";
  position:absolute;
  left:0;
  bottom:-4px;
  width:0%;
  height:2px;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  transition:.3s;
}

nav a:hover::after{
  width:100%;
}

.logonavbar{
  display:flex;
  align-items:center;
  gap:10px;
}

.logonavbar img{
  height:34px;
  width:auto;
}

header{
  text-align:center;
  padding:110px 8% 80px;
  position:relative;
}

header h1{
  font-size:3.2rem;
  background:linear-gradient(90deg,var(--pink),var(--blue));
  -webkit-background-clip:text;
  color:transparent;
}

header p{
  max-width:700px;
  margin:auto;
  opacity:.9;
}

header::after{
  content:"";
  position:absolute;
  width:250px;
  height:250px;
  background:radial-gradient(circle,var(--blue),transparent);
  top:30%;
  left:50%;
  transform:translateX(-50%);
  filter:blur(90px);
  opacity:.25;
}

section{
  padding:80px 8%;
}

h2{
  text-align:center;
  font-size:2.3rem;
  margin-bottom:50px;
}

.card{
  background:var(--glass);
  border-radius:24px;
  padding:30px;
  backdrop-filter:blur(18px);
  transition:.4s;
  opacity:0;
  transform:translateY(40px);
}

.card.visible{
  opacity:1;
  transform:translateY(0);
}

.card:hover{
  transform:translateY(-8px);
  box-shadow:0 0 30px rgba(79,172,254,.4);
}

.logo{
  width:160px;
  display:block;
  margin:0 auto 25px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
  gap:30px;
}

.member{
  background:var(--glass);
  border-radius:24px;
  padding:25px;
  text-align:center;
  backdrop-filter:blur(18px);
  transition:.4s;
  opacity:0;
  transform:translateY(40px);
}

.member.visible{
  opacity:1;
  transform:translateY(0);
}

.member:hover{
  transform:translateY(-10px) scale(1.02);
  box-shadow:0 0 35px rgba(79,172,254,.4);
}

.profile-pic{
  width:120px;
  height:120px;
  border-radius:50%;
  object-fit:cover;
  margin-bottom:15px;
}

.member h3{
  margin:10px 0 5px;
  color:var(--yellow);
}

.job{
  font-size:.95rem;
  opacity:.85;
}

footer{
  text-align:center;
  padding:40px;
  background:#070816;
  opacity:.85;
  margin-top:60px;
}
</style>
</head>

<body>

<nav>
  <div class="logonavbar">
  <img src="laurensialogo.png" alt="Santa Laurensia Logo">
  <strong>PHYSIXLAB</strong>
  </div>
  <div>
    <a href="index.html">Home</a>
    <a href="kinematics.html">Kinematics</a>
    <a href="waves.html">Waves</a>
    <a href="staticelectricity.html">Static Electricity</a>
    <a href="magnetism.html">Magnetism</a>
    <a href="references.html">References</a>
    <a href="about.html">About</a>
  </div>
</nav>

<header>
  <h1>About PHYSIXLAB</h1>
  <p>Learn more about our school and the team behind this educational physics platform.</p>
</header>

<section>
  <h2>Our School</h2>

  <div class="card" style="text-align:justify; max-width:900px; margin:auto;">
    <img src="laurensialogo.png" alt="Santa Laurensia School Logo" class="logo">

    <p>
      Founded in 1994, Santa Laurensia School is a progressive Roman Catholic School committed to nurturing the whole child—mind, body, and spirit.
      We offer a bilingual, holistic education from early years through senior high school, preparing students for both academic excellence and personal growth.
    </p>

    <p>
      Guided by a diverse team of dedicated local and international educators, 
      our community celebrates global awareness and cultural appreciation while remaining 
      rooted in strong Catholic values. At Santa Laurensia, learning is more than academics—it 
      is a joyful journey that cultivates character, perseverance, and faith.
    </p>

    <p>
      Our mission is to empower every student to reach their full potential, 
      compassionate and future-ready leaders who can navigate global challenges 
      with resilience and purpose. 
    </p>
  </div>
</section>

<section>
  <h2>Meet the Team</h2>

  <div class="grid">

    <div class="member">
      <img src="Image_20260226_085854_484.jpeg" class="profile-pic" alt="Celine">
      <h3>Celine Ie</h3>
      <p class="job">Project Leader</p>
    </div>

    <div class="member">
      <img src="IMG_5814.jpeg" class="profile-pic" alt="Jocelyn">
      <h3>Jocelyn Matthea Gunawan</h3>
      <p class="job">Content Specialist</p>
    </div>

    <div class="member">
      <img src="michelle9c.jpeg" class="profile-pic" alt="Michelle">
      <h3>Michelle Aretha Lie</h3>
      <p class="job">Designer & Editor</p>
    </div>

    <div class="member">
      <img src="matthewkusnanto.jpg" class="profile-pic" alt="Matt">
      <h3>Matthew Kusnanto</h3>
      <p class="job">Web Developer</p>
    </div>

  </div>
</section>

<footer>
  PHYSIXLAB — Educational Physics Platform
</footer>

<script>
window.addEventListener("load", () => {
  document.body.classList.add("loaded");
});

const observer = new IntersectionObserver(entries=>{
  entries.forEach(entry=>{
    if(entry.isIntersecting){
      entry.target.classList.add("visible");
    }
  });
},{threshold:0.2});

document.querySelectorAll(".card, .member").forEach(el=>{
  observer.observe(el);
});
</script>

</body>
</html>
