<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Hwa String Quartet — 花·四重奏</title>
<meta name="description" content="Hwa String Quartet — Celebrating diversity through sound.">
<style>
  /* ========== 基础配色与排版 ========== */
  :root{
    --bg:#fffaf8;        /* 奶油白 */
    --accent:#f3d9dd;    /* 粉雾（浅） */
    --muted:#8b8b8b;
    --text:#1b1b1b;
    --gold:#c9a97a;
    --card:#fff;
    --flower-line:#d69aa6;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;color:var(--text);background:linear-gradient(180deg,var(--bg),#fff);-webkit-font-smoothing:antialiased;overflow-x:hidden}
  a{color:inherit}
  .container{max-width:1050px;margin:0 auto;padding:28px}
  header{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:6px 0}
  .brand{display:flex;align-items:center;gap:12px}
  .logo{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#fdeff0);display:flex;align-items:center;justify-content:center;font-weight:700;color:var(--text);border:1px solid rgba(0,0,0,0.04)}
  nav{display:flex;gap:14px}
  nav a{font-weight:600;padding:8px;border-radius:8px;color:var(--muted);text-decoration:none}
  nav a:hover{color:var(--text);background:rgba(0,0,0,0.03)}
  .hero{display:flex;gap:28px;align-items:center;padding:28px 0}
  .hero-left{flex:1}
  h1{margin:0;font-size:34px;line-height:1.05}
  p.lead{color:var(--muted);margin-top:10px;font-size:16px}
  .btn{display:inline-flex;align-items:center;gap:10px;padding:10px 16px;border-radius:12px;background:var(--accent);color:var(--text);font-weight:700;text-decoration:none;box-shadow:0 6px 18px rgba(215,164,174,0.08);border:1px solid rgba(0,0,0,0.02)}
  .btn.secondary{background:transparent;border:1px solid rgba(0,0,0,0.04);color:var(--text);font-weight:600}
  .hero-right{width:360px}
  .card{background:var(--card);border-radius:14px;padding:16px;border:1px solid rgba(0,0,0,0.04);box-shadow:0 6px 18px rgba(0,0,0,0.03)}
  section{padding:34px 0;border-top:1px solid rgba(0,0,0,0.03)}
  .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:18px}
  .member{display:flex;gap:14px;align-items:center}
  .member img{width:96px;height:96px;object-fit:cover;border-radius:10px;border:1px solid rgba(0,0,0,0.04)}
  .meta{color:var(--muted);font-size:14px}
  .venues{display:flex;flex-wrap:wrap;gap:8px}
  .venue{background:rgba(0,0,0,0.02);padding:8px 10px;border-radius:10px;font-size:14px;color:var(--muted)}
  footer{padding:28px 0;color:var(--muted);font-size:14px;text-align:center}

  /* ========== 线条极简花朵图标（SVG 用作背景和按钮） ========== */
  .flower-icon{width:34px;height:34px;display:inline-block;vertical-align:middle}
  .flower-btn{display:inline-flex;align-items:center;gap:10px;padding:8px 12px;border-radius:12px;background:transparent;border:1px solid rgba(0,0,0,0.04);cursor:pointer;transition:transform .28s cubic-bezier(.2,.9,.3,1)}
  .flower-btn:hover{transform:translateY(-4px)}
  /* 花朵 hover 微风摇（利用 transform + CSS variable） */
  .sway{transform-origin:center;transition:transform .6s ease-in-out}

  /* ========== loading overlay: bouquet 展开动画 ========== */
  #loadingOverlay{
    position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:var(--bg);z-index:9999;
  }
  .bouquet-wrap{width:260px;height:260px;display:flex;align-items:center;justify-content:center;position:relative}
  .paper{position:absolute;bottom:0;width:220px;height:140px;border-radius:18px;background:linear-gradient(180deg,#fdeff0,#fff);transform-origin:50% 0%;box-shadow:0 10px 30px rgba(0,0,0,0.06);border:1px solid rgba(0,0,0,0.03);clip-path:polygon(0 0,100% 0,80% 100%,20% 100%)}
  .flower{position:absolute;opacity:0;transform:translateY(30px) scale(.6);transition:all .8s cubic-bezier(.2,.9,.3,1)}
  /* 每朵花的位置（线条风格） */
  .f1{left:50%;top:48%}
  .f2{left:35%;top:42%}
  .f3{left:65%;top:42%}
  .f4{left:45%;top:30%}
  .f5{left:58%;top:28%}
  .bouquet-open .flower{opacity:1;transform:translateY(0) scale(1)}
  /* 花瓣轻微飘落 */
  .petal{position:absolute;width:10px;height:16px;background:var(--accent);opacity:.9;border-radius:6px;transform-origin:center;animation:petal-fall 3.2s linear infinite;pointer-events:none}
  @keyframes petal-fall{
    0%{transform:translateY(0) rotate(0) translateX(0);opacity:1}
    100%{transform:translateY(200px) rotate(360deg) translateX(30px);opacity:0}
  }

  /* ========== 交互动画（按钮点击：旋转 / 绽放 / 微风摇） ========== */
  .flower-rotate{transition:transform .7s cubic-bezier(.17,.67,.32,1)}
  .rotated{transform:rotate(360deg)}
  .bloom .petals{transform:scale(1);transition:transform .45s cubic-bezier(.2,.9,.3,1)}
  .petals{transform:scale(0.01);transform-origin:center}
  .hover-sway:hover .sway{transform:rotate(-6deg)}
  .member-card{display:flex;gap:12px;align-items:flex-start}

  /* ========== responsive ========== */
  @media(max-width:900px){
    .grid{grid-template-columns:1fr}
    .hero{flex-direction:column}
    .hero-right{width:100%}
    nav{display:none}
  }
</style>
</head>
<body>
  <!-- ========== Loading Overlay（花束展开） ========== -->
  <div id="loadingOverlay" aria-hidden="true">
    <div class="bouquet-wrap" id="bouquet">
      <div class="paper" id="paper"></div>

      <!-- 线条极简花朵（SVG） - 花朵位置 f1..f5 -->
      <div class="flower f1" id="flower1">
        <svg class="flower-icon" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" color:var(--flower-line);>
            <circle cx="32" cy="28" r="4" fill="none" stroke="var(--flower-line)"></circle>
            <path d="M32 18c6 0 8 5 8 5s-2 5-8 5-8-5-8-5 2-5 8-5z" stroke="var(--flower-line)"/>
            <path d="M22 26c-4-2-6 2-6 2s3 4 7 3" stroke="var(--flower-line)"/>
            <path d="M42 26c4-2 6 2 6 2s-3 4-7 3" stroke="var(--flower-line)"/>
          </g>
        </svg>
      </div>

      <div class="flower f2" id="flower2">
        <svg class="flower-icon" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
            <path d="M32 22c0 6-6 8-6 8s-2-6 4-10c6-4 8-2 8-2s-4 2-6 4z"/>
            <circle cx="32" cy="28" r="3" fill="none"/>
          </g>
        </svg>
      </div>

      <div class="flower f3" id="flower3">
        <svg class="flower-icon" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
            <path d="M28 20c-4 3-6 6-4 10s6 2 8 0 2-7-4-10z"/>
            <path d="M36 20c4 3 6 6 4 10s-6 2-8 0-2-7 4-10z"/>
            <circle cx="32" cy="30" r="2.5" />
          </g>
        </svg>
      </div>

      <div class="flower f4" id="flower4">
        <svg class="flower-icon" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g stroke="var(--flower-line)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M32 26c-5-4-8-4-10-2s-1 6 3 8 8 2 8 2 4-2 6-4-2-4-7-4z"/>
            <circle cx="33" cy="24" r="2"/>
          </g>
        </svg>
      </div>

      <div class="flower f5" id="flower5">
        <svg class="flower-icon" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
            <path d="M32 20c2 6 6 6 8 6s4-2 4-2-2 6-8 8-10-2-12-6 6-6 8-6z"/>
            <circle cx="32" cy="28" r="2"/>
          </g>
        </svg>
      </div>

      <!-- 柔和飘落的花瓣（几个示意） -->
      <div class="petal" style="left:40%;top:10%;animation-delay:.6s;width:8px;height:12px;background:#f9e6ea"></div>
      <div class="petal" style="left:60%;top:8%;animation-delay:1.2s;width:9px;height:14px;background:#fdeff0"></div>
      <div class="petal" style="left:30%;top:6%;animation-delay:1.6s;width:7px;height:11px;background:#f7dce1"></div>
    </div>
  </div>

  <!-- ========== 主体内容 ========== -->
  <div class="container" id="mainContent" style="opacity:0;transition:opacity .6s ease-in-out">
    <header>
      <div class="brand">
        <div class="logo" aria-hidden="true">
          HWA
        </div>
        <div>
          <div style="font-weight:800">Hwa String Quartet</div>
          <div style="font-size:13px;color:var(--muted)">花 · 四重奏</div>
        </div>
      </div>
      <nav>
        <a href="#about">About</a>
        <a href="#members">Members</a>
        <a href="#appearances">Appearances</a>
        <a href="#contact">Contact</a>
      </nav>
    </header>

    <main>
      <section class="hero" id="home">
        <div class="hero-left">
          <h1>Celebrating diversity through sound</h1>
          <p class="lead">Hwa String Quartet unites musicians from Singapore, Korea, Taiwan, and China — bringing a unique multicultural voice to classical and contemporary repertoire.</p>
          <div style="margin-top:16px">
            <button class="btn flower-btn" id="btnListen">
              <!-- 花朵 icon (线条) -->
              <svg class="flower-icon petals" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M32 22c6 0 8 5 8 5s-2 5-8 5-8-5-8-5 2-5 8-5z"/>
                  <circle cx="32" cy="30" r="3" />
                </g>
              </svg>
              Listen
            </button>
            <a class="btn secondary" href="#contact" style="margin-left:12px">Book a performance</a>
          </div>
          <div style="margin-top:14px;color:var(--muted);font-size:13px">
            成立：2025 · 语言：英文 / 中文 · Email: <a href="mailto:hwa.quartet@gmail.com">hwa.quartet@gmail.com</a>
          </div>
        </div>

        <div class="hero-right">
          <div class="card">
            <strong>Quick info</strong>
            <div style="margin-top:10px;color:var(--muted);font-size:14px">
              四位成员来自新加坡 / 韩国 / 台湾 / 中国<br>
              Mentorship：Sinfonia Smith Square<br>
              Repertoire：Classical — Contemporary — Arrangements
            </div>
            <div style="margin-top:12px">
              <button class="btn flower-btn hover-sway" id="btnAbout">
                <svg class="flower-icon sway" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M32 22c6 0 8 5 8 5s-2 5-8 5-8-5-8-5 2-5 8-5z"/>
                    <circle cx="32" cy="30" r="3" />
                  </g>
                </svg>
                About
              </button>
            </div>
          </div>
        </div>
      </section>

      <!-- ========== About（引用你上传的 CV 内容） ========== -->
      <section id="about">
        <h2>About Hwa Quartet</h2>
        <p style="color:var(--muted);max-width:780px">
          Formed in 2025, the Hwa String Quartet unites four musicians from Singapore, Korea, Taiwan, and China. The word <strong>Hwa (花)</strong> means 'flower' across many Asian cultures — symbolising beauty, growth, and connection. The quartet celebrates cultural diversity and musical collaboration, performing repertoire that ranges from classical masterpieces to contemporary works and innovative arrangements. （内容来自你提供的 CV。） [oai_citation:1‡Hwa Quartet CV.pdf](sediment://file_0000000077c072469528bd184c680463)
        </p>
      </section>

      <!-- ========== Members（每位成员卡片） ========== -->
      <section id="members">
        <h2>Members</h2>
        <div class="grid" style="margin-top:14px">
          <!-- Member 1 -->
          <div class="card member-card">
            <img src="https://via.placeholder.com/240x240.png?text=Xiongyufan+Miao" alt="Xiongyufan Miao">
            <div>
              <div style="font-weight:700">Violin — Xiongyufan Miao</div>
              <div class="meta" style="margin-top:6px">
                Royal Academy of Music — Master of Arts, LRAM<br>
                Royal Conservatoire of Scotland — Bachelor of Music (Honours)<br>
                Former Co-Leader of National Youth Orchestra of Scotland; participant of RSNO & Scottish Ensemble side-by-side project.
              </div>
            </div>
          </div>

          <!-- Member 2 -->
          <div class="card member-card">
            <img src="https://via.placeholder.com/240x240.png?text=Yoonseo+Oh" alt="Yoonseo Oh">
            <div>
              <div style="font-weight:700">Violin — Yoonseo Oh</div>
              <div class="meta" style="margin-top:6px">
                Texas Christian University — Master of Music<br>
                Royal Conservatoire of Scotland — Bachelor of Music (Honours)<br>
                Extra with London Philharmonic Orchestra, The Hallé, Royal Philharmonic Orchestra; fellow of Sinfonia Smith Square.
              </div>
            </div>
          </div>

          <!-- Member 3 -->
          <div class="card member-card">
            <img src="https://via.placeholder.com/240x240.png?text=Jasmine+Ong" alt="Jasmine Ong">
            <div>
              <div style="font-weight:700">Viola — Jasmine Ong</div>
              <div class="meta" style="margin-top:6px">
                Royal College of Music — Master of Performance (Distinction)<br>
                Royal Birmingham Conservatoire — Bachelor of Music (First Class Honours)<br>
                Former participant of Ulster Orchestra Professional Experience Scheme; fellow of Sinfonia Smith Square.
              </div>
            </div>
          </div>

          <!-- Member 4 -->
          <div class="card member-card">
            <img src="https://via.placeholder.com/240x240.png?text=Chian-Chian+Hsu" alt="Chian-Chian Hsu">
            <div>
              <div style="font-weight:700">Cello — Chian-Chian Hsu</div>
              <div class="meta" style="margin-top:6px">
                （详情见 CV）<br>
                Members receive mentorship through Sinfonia Smith Square, and have received coaching across more than a dozen chamber groups.  [oai_citation:2‡Hwa Quartet CV.pdf](sediment://file_0000000077c072469528bd184c680463)
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- ========== Appearances / Venues ========== -->
      <section id="appearances">
        <h2>Selected Appearances & Venues</h2>
        <p style="color:var(--muted);margin-top:6px">Collective experience includes performances across the United Kingdom, Asia, and internationally (selected venues listed below).  [oai_citation:3‡Hwa Quartet CV.pdf](sediment://file_0000000077c072469528bd184c680463)</p>
        <div style="margin-top:12px" class="venues">
          <!-- UK -->
          <div class="venue">Buckingham Palace</div>
          <div class="venue">Royal Festival Hall</div>
          <div class="venue">Wigmore Hall</div>
          <div class="venue">London Coliseum</div>
          <div class="venue">City Halls Glasgow</div>
          <div class="venue">Glasgow Royal Concert Hall</div>
          <div class="venue">Usher Hall</div>

          <!-- Asia -->
          <div class="venue">Shanghai Oriental Art Centre</div>
          <div class="venue">Beijing NCPA</div>
          <div class="venue">Esplanade Concert Hall</div>
          <div class="venue">Tokyo Opera City</div>
          <div class="venue">Cultural Centre of the Philippines</div>

          <!-- Others -->
          <div class="venue">Konserthuset Stockholm</div>
          <div class="venue">Arena Shakespeare, Parma</div>
          <div class="venue">Auckland Town Hall</div>
          <div class="venue">Church of Nativity (Bethlehem)</div>
        </div>
      </section>

      <!-- ========== Contact ========== -->
      <section id="contact">
        <h2>Contact</h2>
        <div style="display:grid;grid-template-columns:1fr 320px;gap:18px;margin-top:12px">
          <div>
            <p style="color:var(--muted)">For bookings, press and collaborations, please contact:</p>
            <div class="card">
              <strong>Hwa String Quartet</strong>
              <div style="margin-top:8px;color:var(--muted)">Email: <a href="mailto:hwa.quartet@gmail.com">hwa.quartet@gmail.com</a></div>
              <div style="margin-top:8px;color:var(--muted)">Instagram: <a href="https://instagram.com/hwa.quartet" target="_blank" rel="noopener">@hwa.quartet</a></div>
              <div style="margin-top:12px">
                <a class="btn" href="mailto:hwa.quartet@gmail.com?subject=Booking%20Enquiry">Email us</a>
                <button class="btn secondary flower-btn" id="btnTour" style="margin-left:8px">
                  <svg class="flower-icon sway" viewBox="0 0 64 64"><g stroke="var(--flower-line)" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M32 22c6 0 8 5 8 5s-2 5-8 5-8-5-8-5 2-5 8-5z"/><circle cx="32" cy="30" r="3" /></g></svg>
                  Tour dates
                </button>
              </div>
            </div>
          </div>

          <div>
            <div class="card">
              <strong>Quick CV</strong>
              <div style="margin-top:8px;color:var(--muted);font-size:14px">
                Formed in 2025; members trained at top conservatoires (Royal Academy of Music, Royal College of Music, Royal Conservatoire of Scotland, Royal Birmingham Conservatoire). Mentorship through Sinfonia Smith Square. （完整 CV 已由你上传并使用于本站。） [oai_citation:4‡Hwa Quartet CV.pdf](sediment://file_0000000077c072469528bd184c680463)
              </div>
            </div>
          </div>
        </div>
      </section>

    </main>

    <footer>
      © <span id="year"></span> Hwa String Quartet · Made with ♥ · 粉雾 & 奶油白主题
    </footer>
  </div>

<script>
  // 年份
  document.getElementById('year').textContent = new Date().getFullYear();

  // ======= Loading 动画逻辑（花朵一朵朵展开） =======
  const overlay = document.getElementById('loadingOverlay');
  const bouquet = document.getElementById('bouquet');
  const flowers = Array.from(document.querySelectorAll('.flower'));
  const paper = document.getElementById('paper');

  // 模拟加载：依次展开花朵
  function openBouquetThenReveal(){
    // 1) 微微抬起纸（模拟打开）
    paper.style.transform = 'translateY(-24px) rotate(-2deg) scale(1.02)';
    // 2) 依次显示每朵花（延迟）
    flowers.forEach((f, i) => {
      setTimeout(()=> {
        f.classList.add('visible');
        // add class to wrapper to trigger CSS transition
        bouquet.classList.add('bouquet-open');
      }, 350 + i*250);
    });
    // 3) 等待一会隐藏 overlay
    setTimeout(()=> {
      overlay.style.opacity = '0';
      overlay.style.pointerEvents = 'none';
      document.getElementById('mainContent').style.opacity = '1';
      setTimeout(()=> overlay.remove(), 900);
    }, 1800 + flowers.length * 220);
  }

  // 页面加载后触发（也可替换为 window.onload）
  window.addEventListener('load', ()=> {
    // small delay to let paint settle
    setTimeout(openBouquetThenReveal, 280);
  });

  // ======= 花朵按钮交互 =======
  // Listen 按钮：点击旋转 + 花瓣绽放
  const btnListen = document.getElementById('btnListen');
  btnListen.addEventListener('click', (e)=>{
    const icon = btnListen.querySelector('.petals');
    // rotate
    icon.classList.toggle('rotated');
    // bloom effect on the whole button (toggle class that scales petals)
    btnListen.classList.add('bloom');
    setTimeout(()=> btnListen.classList.remove('bloom'), 700);
    // (示意) 可以在这里打开音频/视频播放器或滚到某个区
    document.location.hash = '#appearances';
  });

  // About 按钮：微风摇
  document.getElementById('btnAbout').addEventListener('click', ()=>{
    document.location.hash = '#about';
  });

  // Tour 按钮：点击弹出简单提示（你可替换为 modal）
  document.getElementById('btnTour').addEventListener('click', (e)=>{
    // 旋转 icon
    const ic = e.currentTarget.querySelector('.sway');
    ic.classList.toggle('rotated');
    alert('Tour dates will be added here. 如果你有巡演日期，请把信息发给我，我可以把它们加入页面。');
  });

  // 小动画：让花朵在鼠标 hover 时微微旋转（delegation）
  document.querySelectorAll('.flower-btn').forEach(btn=>{
    btn.addEventListener('mouseenter', ()=> {
      const s = btn.querySelector('.sway');
      if(s) s.style.transform = 'rotate(-6deg)';
    });
    btn.addEventListener('mouseleave', ()=> {
      const s = btn.querySelector('.sway');
      if(s) s.style.transform = 'rotate(0deg)';
    });
  });

  // 为 loading 的每朵花设置 stagger 动画（以 class 控制最终样式）
  // 但因为我们用的 CSS transition，已经通过 bouquet-open 统一触发

  // 防止用户按回退时 overlay 未移除
  if(document.readyState === 'complete'){
    // already loaded
    setTimeout(()=> {
      if(document.getElementById('loadingOverlay')) openBouquetThenReveal();
    }, 120);
  }
</script>
</body>
</html>
