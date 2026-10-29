<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Brightloom Studio</title>
<style>
  /* ========== COLOURS (change these to retheme the site) ========== */
  :root{
    --pink:#ff4d8d; --purple:#7c4dff; --blue:#27c4ff; --yellow:#ffd23f; --green:#2de2a6;
    --bg:#ffffff; --bg2:#f3efff; --text:#222; --muted:#666; --card:#ffffff; --line:#e4ddff;
  }
  body.dark{
    --bg:#120d26; --bg2:#1b1538; --text:#f2f0ff; --muted:#b4aedb; --card:#241c47; --line:#3a2f6e;
  }

  /* ========== BASE ========== */
  *{margin:0;padding:0;box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--text);line-height:1.6;text-align:center;transition:background .4s,color .4s}
  a{text-decoration:none;color:inherit}
  section{padding:80px 5%}
  h2{color:var(--purple);font-size:2rem;margin-bottom:10px}
  .sub{color:var(--muted);margin-bottom:40px}
  .btn{display:inline-block;background:var(--yellow);color:#222;padding:14px 30px;border-radius:30px;font-weight:bold;border:none;font-size:16px;cursor:pointer;transition:transform .3s}
  .btn:hover{transform:scale(1.08)}
  .btn.purple{background:var(--purple);color:#fff}

  /* scroll progress bar */
  #bar{position:fixed;top:0;left:0;height:4px;width:0;background:linear-gradient(90deg,var(--pink),var(--blue));z-index:100}

  /* ========== MENU ========== */
  nav{display:flex;justify-content:space-between;align-items:center;padding:14px 5%;background:var(--bg);box-shadow:0 2px 10px rgba(0,0,0,.12);position:sticky;top:0;z-index:50}
  .logo{font-size:24px;font-weight:bold;color:var(--purple)}
  .links{display:flex;align-items:center;gap:22px}
  .links a{font-weight:bold;color:var(--muted)}
  .links a:hover,.links a.active{color:var(--pink)}
  #theme{background:var(--bg2);border:none;width:40px;height:40px;border-radius:50%;font-size:18px;cursor:pointer}
  #burger{display:none;background:none;border:none;font-size:28px;color:var(--text);cursor:pointer}

  /* ========== HERO ========== */
  .hero{background:linear-gradient(135deg,var(--pink),var(--purple),var(--blue));background-size:200% 200%;animation:move 8s ease infinite;color:#fff;padding:110px 5%}
  @keyframes move{50%{background-position:100% 100%}}
  .hero h1{font-size:2.8rem;margin-bottom:14px}
  .hero p{font-size:1.2rem;margin-bottom:28px}
  #typed{color:var(--yellow);border-right:3px solid var(--yellow);padding-right:4px}

  /* ========== STATS ========== */
  .stats{display:flex;flex-wrap:wrap;gap:20px;justify-content:center;background:var(--bg2)}
  .stat{flex:1;min-width:150px}
  .stat b{font-size:2.8rem;color:var(--pink);display:block}

  /* ========== CARDS (services) ========== */
  .cards{display:flex;gap:20px;flex-wrap:wrap;justify-content:center}
  .card{flex:1;min-width:230px;padding:30px;border-radius:16px;color:#fff;transition:transform .3s,box-shadow .3s;cursor:default}
  .card:hover{transform:translateY(-10px);box-shadow:0 15px 30px rgba(124,77,255,.35)}
  .card h1{font-size:2.6rem}

  /* ========== PORTFOLIO ========== */
  .filters{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-bottom:30px}
  .filter{padding:10px 22px;border-radius:30px;border:2px solid var(--purple);background:none;color:var(--purple);font-weight:bold;cursor:pointer}
  .filter.on,.filter:hover{background:var(--purple);color:#fff}
  .works{display:flex;flex-wrap:wrap;gap:20px;justify-content:center}
  .work{flex:1 1 260px;max-width:340px;height:200px;border-radius:16px;color:#fff;display:flex;flex-direction:column;justify-content:flex-end;align-items:flex-start;padding:20px;cursor:pointer;position:relative;transition:transform .3s}
  .work:hover{transform:scale(1.05)}
  .work span{position:absolute;top:14px;right:18px;font-size:3rem}
  .work.hide{display:none}

  /* ========== POP-UP (modal) ========== */
  #modal{position:fixed;inset:0;background:rgba(0,0,0,.65);display:none;align-items:center;justify-content:center;z-index:200;padding:20px}
  #modal.open{display:flex}
  .modal-box{background:var(--card);color:var(--text);padding:36px;border-radius:20px;max-width:440px;width:100%;position:relative;animation:pop .3s}
  @keyframes pop{from{transform:scale(.8);opacity:0}}
  .modal-box .x{position:absolute;top:10px;right:16px;font-size:30px;cursor:pointer;color:var(--muted)}
  #mIcon{font-size:3.5rem}

  /* ========== PRICE CALCULATOR ========== */
  .calc{max-width:620px;margin:auto;background:var(--bg2);border:1px solid var(--line);border-radius:20px;padding:34px;text-align:left}
  .calc label{display:block;font-weight:bold;margin:16px 0 6px}
  .calc select{width:100%;padding:12px;border-radius:10px;border:2px solid var(--line);background:var(--card);color:var(--text);font-size:1rem}
  .calc input[type=range]{width:100%;accent-color:var(--pink)}
  .checks{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:8px}
  .checks label{font-weight:normal;margin:0;background:var(--card);padding:10px;border-radius:10px;border:1px solid var(--line);cursor:pointer}
  .total{text-align:center;margin-top:26px;font-size:1.1rem}
  .total b{display:block;font-size:2.8rem;color:var(--pink)}

  /* ========== TESTIMONIAL SLIDER ========== */
  .slider{max-width:650px;margin:auto;overflow:hidden;border-radius:20px}
  .track{display:flex;transition:transform .6s}
  .slide{min-width:100%;padding:36px;background:var(--bg2);border:1px solid var(--line);border-radius:20px}
  .slide .stars{color:var(--yellow);font-size:1.3rem;letter-spacing:3px}
  .dots{margin-top:18px}
  .dots button{width:12px;height:12px;border-radius:50%;border:none;background:var(--line);margin:0 5px;cursor:pointer}
  .dots button.on{background:var(--pink);width:30px;border-radius:10px}

  /* ========== FAQ ========== */
  .faq{max-width:700px;margin:auto;text-align:left}
  .q{background:var(--bg2);border:1px solid var(--line);border-radius:12px;margin-bottom:12px;overflow:hidden}
  .q button{width:100%;padding:18px 20px;background:none;border:none;text-align:left;font-size:1rem;font-weight:bold;color:var(--text);cursor:pointer;display:flex;justify-content:space-between}
  .q .ans{max-height:0;overflow:hidden;padding:0 20px;color:var(--muted);transition:max-height .4s,padding .4s}
  .q.open .ans{max-height:150px;padding:0 20px 18px}
  .q.open button span{transform:rotate(45deg)}
  .q button span{transition:.3s;color:var(--pink);font-size:1.4rem}

  /* ========== CONTACT FORM ========== */
  form{max-width:480px;margin:auto;display:grid;gap:12px;text-align:left}
  input,textarea{padding:14px;border:2px solid var(--line);border-radius:10px;font-size:1rem;font-family:inherit;background:var(--card);color:var(--text)}
  input:focus,textarea:focus{outline:none;border-color:var(--pink)}
  .bad{border-color:#ff3b3b !important}
  .err{color:#ff3b3b;font-size:.85rem;display:none}
  #count{text-align:right;font-size:.8rem;color:var(--muted)}

  footer{background:#1a1433;color:#fff;padding:30px 5%}

  /* ========== EXTRAS: toast, back-to-top, reveal ========== */
  #toast{position:fixed;bottom:30px;left:50%;transform:translate(-50%,120px);background:var(--green);color:#063;padding:14px 28px;border-radius:30px;font-weight:bold;transition:.5s;z-index:300}
  #toast.show{transform:translate(-50%,0)}
  #top{position:fixed;right:22px;bottom:22px;width:48px;height:48px;border-radius:50%;border:none;background:var(--purple);color:#fff;font-size:20px;cursor:pointer;opacity:0;pointer-events:none;transition:.4s;z-index:90}
  #top.show{opacity:1;pointer-events:auto}
  .reveal{opacity:0;transform:translateY(40px);transition:opacity .8s,transform .8s}
  .reveal.in{opacity:1;transform:none}

  /* ========== PHONES ========== */
  @media(max-width:720px){
    #burger{display:block}
    .links{position:fixed;top:0;right:-100%;height:100vh;width:70%;background:var(--bg2);flex-direction:column;justify-content:center;transition:.4s;box-shadow:-5px 0 20px rgba(0,0,0,.2)}
    .links.open{right:0}
    .hero h1{font-size:2rem}
    .checks{grid-template-columns:1fr}
  }
</style>
</head>
<body>

<div id="bar"></div>

<!-- ============ MENU ============ -->
<nav>
  <span class="logo">✦ Brightloom</span>
  <div class="links" id="menu">
    <a href="#services">Services</a>
    <a href="#work">Work</a>
    <a href="#calculator">Pricing</a>
    <a href="#faq">FAQ</a>
    <a href="#contact">Contact</a>
    <button id="theme" title="Dark / light mode">🌙</button>
  </div>
  <button id="burger">☰</button>
</nav>

<!-- ============ HERO ============ -->
<div class="hero">
  <h1>We Build Websites That <span id="typed"></span></h1>
  <p>Beautiful. Fast. Easy to use.</p>
  <a href="#calculator"><button class="btn">Get an Instant Quote</button></a>
</div>

<!-- ============ STATS (animated counters) ============ -->
<section class="stats">
  <div class="stat"><b class="count" data-to="150">0</b>Projects Done</div>
  <div class="stat"><b class="count" data-to="98">0</b>% Happy Clients</div>
  <div class="stat"><b class="count" data-to="8">0</b>Years Experience</div>
  <div class="stat"><b class="count" data-to="25">0</b>Industries</div>
</section>

<!-- ============ SERVICES ============ -->
<section id="services">
  <h2 class="reveal">Our Services</h2>
  <p class="sub reveal">Everything you need to shine online</p>
  <div class="cards">
    <div class="card reveal" style="background:var(--pink)"><h1>🎨</h1><h3>Web Design</h3><p>Eye-catching designs that match your brand.</p></div>
    <div class="card reveal" style="background:var(--purple)"><h1>💻</h1><h3>Development</h3><p>Fast, mobile-friendly websites.</p></div>
    <div class="card reveal" style="background:var(--blue)"><h1>🛒</h1><h3>Online Stores</h3><p>Sell your products online easily.</p></div>
  </div>
</section>

<!-- ============ PORTFOLIO (filter + pop-up) ============ -->
<section id="work" style="background:var(--bg2)">
  <h2 class="reveal">Our Work</h2>
  <p class="sub reveal">Click any project to see details</p>
  <div class="filters">
    <button class="filter on" data-f="all">All</button>
    <button class="filter" data-f="shop">Stores</button>
    <button class="filter" data-f="business">Business</button>
    <button class="filter" data-f="creative">Creative</button>
  </div>
  <div class="works">
    <div class="work" data-c="shop" data-t="StyleNest" data-d="A fashion store with 500+ products, UPI payments and fast checkout. Sales grew 3x in 4 months." style="background:linear-gradient(135deg,#ff4d8d,#ff8a3d)"><span>👗</span><h3>StyleNest</h3><small>Online Store</small></div>
    <div class="work" data-c="business" data-t="CarePlus Clinic" data-d="A clean clinic website with online appointment booking. Phone calls dropped and bookings doubled." style="background:linear-gradient(135deg,#7c4dff,#27c4ff)"><span>🏥</span><h3>CarePlus Clinic</h3><small>Business Website</small></div>
    <div class="work" data-c="creative" data-t="Lens &amp; Light" data-d="A photographer's portfolio with a full-screen gallery and a simple booking form." style="background:linear-gradient(135deg,#2de2a6,#27c4ff)"><span>📸</span><h3>Lens &amp; Light</h3><small>Portfolio</small></div>
    <div class="work" data-c="shop" data-t="Brew Bros" data-d="A coffee subscription store with monthly plans, order tracking and a loyalty programme." style="background:linear-gradient(135deg,#ffb347,#ff4d8d)"><span>☕</span><h3>Brew Bros</h3><small>Online Store</small></div>
    <div class="work" data-c="business" data-t="GreenRoots Farm" data-d="An organic farm brand site with a product catalogue and WhatsApp ordering." style="background:linear-gradient(135deg,#2de2a6,#7c4dff)"><span>🌱</span><h3>GreenRoots Farm</h3><small>Business Website</small></div>
    <div class="work" data-c="creative" data-t="Beatbox Academy" data-d="A music school website with class schedules, instructor profiles and video previews." style="background:linear-gradient(135deg,#ff4d8d,#7c4dff)"><span>🎵</span><h3>Beatbox Academy</h3><small>Creative</small></div>
  </div>
</section>

<!-- ============ PRICE CALCULATOR ============ -->
<section id="calculator">
  <h2 class="reveal">Instant Price Calculator</h2>
  <p class="sub reveal">Build your package and see the estimate live</p>
  <div class="calc reveal">
    <label for="type">What kind of website?</label>
    <select id="type">
      <option value="10000">Business Website (₹10,000)</option>
      <option value="8000">Portfolio (₹8,000)</option>
      <option value="30000">Online Store (₹30,000)</option>
    </select>

    <label for="pages">Number of pages: <span id="pagesVal">5</span></label>
    <input type="range" id="pages" min="1" max="20" value="5">

    <label>Extras</label>
    <div class="checks">
      <label><input type="checkbox" value="5000"> 🔍 SEO (+₹5,000)</label>
      <label><input type="checkbox" value="4000"> 🎨 Logo design (+₹4,000)</label>
      <label><input type="checkbox" value="6000"> 📝 Blog (+₹6,000)</label>
      <label><input type="checkbox" value="3000"> 🛠️ Maintenance (+₹3,000)</label>
    </div>

    <div class="total">Estimated price<b id="total">₹0</b></div>
    <div style="text-align:center"><button class="btn purple" id="useQuote">Request This Quote</button></div>
  </div>
</section>

<!-- ============ TESTIMONIALS ============ -->
<section style="background:var(--bg2)">
  <h2 class="reveal">What Clients Say</h2>
  <p class="sub reveal">Real words from happy businesses</p>
  <div class="slider">
    <div class="track" id="track">
      <div class="slide"><div class="stars">★★★★★</div><p>“Our new website doubled our enquiries in two months. Creative, quick and always available.”</p><br><b>Priya Raman</b><br><small>Founder, StyleNest</small></div>
      <div class="slide"><div class="stars">★★★★★</div><p>“They understood our brand perfectly. Patients can now book appointments in seconds.”</p><br><b>Dr. Karthik S.</b><br><small>Director, CarePlus Clinic</small></div>
      <div class="slide"><div class="stars">★★★★★</div><p>“Professional, transparent and fun to work with. Delivered before the deadline.”</p><br><b>Arjun Mehta</b><br><small>Owner, Brew Bros</small></div>
    </div>
  </div>
  <div class="dots" id="dots"></div>
</section>

<!-- ============ FAQ (accordion) ============ -->
<section id="faq">
  <h2 class="reveal">Frequently Asked Questions</h2>
  <p class="sub reveal">Click a question to open the answer</p>
  <div class="faq">
    <div class="q"><button>How long does a website take? <span>+</span></button><div class="ans">A simple website takes 1 to 2 weeks. Online stores take 3 to 5 weeks depending on the number of products.</div></div>
    <div class="q"><button>Will my website work on mobile phones? <span>+</span></button><div class="ans">Yes. Every website we build is fully responsive and looks great on phones, tablets and computers.</div></div>
    <div class="q"><button>Do you provide support after launch? <span>+</span></button><div class="ans">Yes. We offer free support for the first month and affordable maintenance plans after that.</div></div>
    <div class="q"><button>Can I update the website myself? <span>+</span></button><div class="ans">Absolutely. We can set it up so you can change text, images and products without any coding.</div></div>
  </div>
</section>

<!-- ============ CONTACT ============ -->
<section id="contact" style="background:var(--bg2)">
  <h2 class="reveal">Contact Us</h2>
  <p class="sub reveal">Tell us about your idea. We reply within 24 hours.</p>
  <form id="form" novalidate>
    <input type="text" id="name" placeholder="Your name">
    <span class="err">Please enter your name</span>
    <input type="email" id="email" placeholder="Your email">
    <span class="err">Please enter a valid email</span>
    <textarea id="msg" rows="5" maxlength="300" placeholder="Tell us about your project"></textarea>
    <div id="count">0 / 300</div>
    <span class="err">Please write a short message</span>
    <button class="btn purple" type="submit">Send Message 🚀</button>
  </form>
</section>

<footer>© <span id="year"></span> Brightloom Studio · hello@brightloom.studio</footer>

<!-- pop-up, toast and back-to-top button -->
<div id="modal"><div class="modal-box"><span class="x" id="close">×</span><div id="mIcon"></div><h2 id="mTitle"></h2><p id="mText"></p><br><a href="#contact" id="mBtn"><button class="btn purple">Start a Similar Project</button></a></div></div>
<div id="toast"></div>
<button id="top">↑</button>

<script>
  // short helpers
  const $  = s => document.querySelector(s);
  const $$ = s => document.querySelectorAll(s);

  // ---------- 1. Footer year ----------
  $('#year').textContent = new Date().getFullYear();

  // ---------- 2. Typing effect in the hero ----------
  const words = ['grow your business.', 'look amazing.', 'load super fast.', 'win customers.'];
  let w = 0, c = 0, deleting = false;
  function type(){
    const word = words[w];
    $('#typed').textContent = word.slice(0, c);
    if(!deleting && c < word.length){ c++; setTimeout(type, 90); }
    else if(!deleting){ deleting = true; setTimeout(type, 1400); }
    else if(c > 0){ c--; setTimeout(type, 45); }
    else { deleting = false; w = (w + 1) % words.length; setTimeout(type, 300); }
  }
  type();

  // ---------- 3. Dark / light mode ----------
  $('#theme').onclick = () => {
    const dark = document.body.classList.toggle('dark');
    $('#theme').textContent = dark ? '☀️' : '🌙';
  };

  // ---------- 4. Mobile menu ----------
  $('#burger').onclick = () => $('#menu').classList.toggle('open');
  $$('#menu a').forEach(a => a.onclick = () => $('#menu').classList.remove('open'));

  // ---------- 5. Scroll: progress bar, back-to-top, active link ----------
  const sections = [...$$('section[id]')];
  addEventListener('scroll', () => {
    const y = scrollY, max = document.documentElement.scrollHeight - innerHeight;
    $('#bar').style.width = (y / max * 100) + '%';
    $('#top').classList.toggle('show', y > 500);
    let current = '';
    sections.forEach(s => { if(y >= s.offsetTop - 150) current = s.id; });
    $$('#menu a').forEach(a => a.classList.toggle('active', a.getAttribute('href') === '#' + current));
  });
  $('#top').onclick = () => scrollTo({top: 0, behavior: 'smooth'});

  // ---------- 6. Reveal on scroll + animated counters ----------
  const watcher = new IntersectionObserver(items => {
    items.forEach(item => {
      if(!item.isIntersecting) return;
      item.target.classList.add('in');
      if(item.target.classList.contains('count')) countUp(item.target);
      watcher.unobserve(item.target);
    });
  }, {threshold: .2});
  $$('.reveal, .count').forEach(el => watcher.observe(el));

  function countUp(el){
    const to = +el.dataset.to; let n = 0;
    const timer = setInterval(() => {
      n += Math.ceil(to / 50);
      if(n >= to){ n = to; clearInterval(timer); }
      el.textContent = n;
    }, 30);
  }

  // ---------- 7. Portfolio filter ----------
  $$('.filter').forEach(btn => btn.onclick = () => {
    $$('.filter').forEach(b => b.classList.remove('on'));
    btn.classList.add('on');
    $$('.work').forEach(card => {
      card.classList.toggle('hide', btn.dataset.f !== 'all' && card.dataset.c !== btn.dataset.f);
    });
  });

  // ---------- 8. Portfolio pop-up ----------
  $$('.work').forEach(card => card.onclick = () => {
    $('#mIcon').textContent  = card.querySelector('span').textContent;
    $('#mTitle').innerHTML   = card.dataset.t;
    $('#mText').textContent  = card.dataset.d;
    $('#modal').classList.add('open');
  });
  const closeModal = () => $('#modal').classList.remove('open');
  $('#close').onclick = closeModal;
  $('#mBtn').onclick  = closeModal;
  $('#modal').onclick = e => { if(e.target.id === 'modal') closeModal(); };
  addEventListener('keydown', e => { if(e.key === 'Escape') closeModal(); });

  // ---------- 9. Price calculator ----------
  const PER_PAGE = 1000;           // price for each page
  function calculate(){
    const base  = +$('#type').value;
    const pages = +$('#pages').value;
    let extras = 0;
    $$('.checks input:checked').forEach(box => extras += +box.value);
    const total = base + pages * PER_PAGE + extras;
    $('#pagesVal').textContent = pages;
    $('#total').textContent = '₹' + total.toLocaleString('en-IN');
    return total;
  }
  $('#type').onchange = calculate;
  $('#pages').oninput = calculate;
  $$('.checks input').forEach(b => b.onchange = calculate);
  calculate();

  $('#useQuote').onclick = () => {
    const kind = $('#type').selectedOptions[0].text.split(' (')[0];
    $('#msg').value = `Hi! I'd like a quote for a ${kind} with ${$('#pages').value} pages. Estimated budget: ${$('#total').textContent}.`;
    $('#count').textContent = $('#msg').value.length + ' / 300';
    $('#contact').scrollIntoView({behavior: 'smooth'});
    toast('Quote added to the contact form ✔');
  };

  // ---------- 10. Testimonial slider ----------
  const slides = $$('.slide'); let s = 0, timer;
  slides.forEach((_, i) => {
    const dot = document.createElement('button');
    dot.onclick = () => { show(i); restart(); };
    $('#dots').appendChild(dot);
  });
  function show(i){
    s = (i + slides.length) % slides.length;
    $('#track').style.transform = `translateX(-${s * 100}%)`;
    [...$('#dots').children].forEach((d, k) => d.classList.toggle('on', k === s));
  }
  function restart(){ clearInterval(timer); timer = setInterval(() => show(s + 1), 4500); }
  show(0); restart();

  // ---------- 11. FAQ accordion ----------
  $$('.q button').forEach(btn => btn.onclick = () => {
    const box = btn.parentElement, wasOpen = box.classList.contains('open');
    $$('.q').forEach(q => q.classList.remove('open'));   // close all
    if(!wasOpen) box.classList.add('open');              // open clicked one
  });

  // ---------- 12. Contact form: live counter + validation ----------
  $('#msg').oninput = () => $('#count').textContent = $('#msg').value.length + ' / 300';

  function check(id, ok){
    const field = $('#' + id);
    field.classList.toggle('bad', !ok);
    let note = field.nextElementSibling;
    if(id === 'msg') note = $('#count').nextElementSibling;   // message error sits after the counter
    note.style.display = ok ? 'none' : 'block';
    return ok;
  }
  $('#form').onsubmit = e => {
    e.preventDefault();
    const a = check('name',  $('#name').value.trim().length > 1);
    const b = check('email', /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test($('#email').value));
    const d = check('msg',   $('#msg').value.trim().length > 5);
    if(a && b && d){
      toast('🎉 Thank you! We will contact you soon.');
      e.target.reset();
      $('#count').textContent = '0 / 300';
    }
  };

  // ---------- 13. Toast message ----------
  function toast(text){
    $('#toast').textContent = text;
    $('#toast').classList.add('show');
    setTimeout(() => $('#toast').classList.remove('show'), 3000);
  }
</script>
</body>
</html>
