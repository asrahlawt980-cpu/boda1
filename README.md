<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Boda | مترجم</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&family=Outfit:wght@300;400;600;700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080b11;
    --surface: #0f1521;
    --surface2: #151d2e;
    --border: #1a2540;
    --accent: #3b82f6;
    --accent2: #60a5fa;
    --accent3: #93c5fd;
    --glow: rgba(59,130,246,0.10);
    --text: #e2e8f0;
    --text-dim: #94a3b8;
    --text-muted: #4a5a72;
    --success: #10b981;
    --r: 14px;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:var(--bg); color:var(--text); font-family:'Cairo','Outfit',sans-serif; min-height:100vh; overflow-x:hidden; }

  .orb { position:fixed; pointer-events:none; z-index:0; border-radius:50%; filter:blur(120px); }
  .orb1 { width:700px; height:700px; background:radial-gradient(circle,rgba(59,130,246,0.07) 0%,transparent 70%); top:-200px; right:-150px; }
  .orb2 { width:500px; height:500px; background:radial-gradient(circle,rgba(99,102,241,0.05) 0%,transparent 70%); bottom:-100px; left:-100px; }

  header {
    position:sticky; top:0; z-index:100;
    background:rgba(8,11,17,0.88);
    backdrop-filter:blur(18px);
    border-bottom:1px solid var(--border);
    padding:0 36px; height:64px;
    display:flex; align-items:center; justify-content:space-between;
  }
  .logo { display:flex; align-items:center; gap:12px; }
  .logo-mark {
    width:40px; height:40px;
    background:linear-gradient(135deg,var(--accent),#6366f1);
    border-radius:11px;
    display:flex; align-items:center; justify-content:center;
    font-size:1.2rem;
    box-shadow:0 4px 18px rgba(59,130,246,0.35);
  }
  .logo-text { font-size:1.4rem; font-weight:900; color:var(--text); font-family:'Outfit',sans-serif; letter-spacing:-1px; }
  .logo-text em { font-style:normal; color:var(--accent2); }
  .logo-sub { font-family:'Outfit',sans-serif; font-size:0.68rem; color:var(--text-muted); letter-spacing:2px; text-transform:uppercase; }
  .badge { display:flex; align-items:center; gap:7px; padding:6px 13px; background:var(--surface); border:1px solid var(--border); border-radius:20px; font-size:0.73rem; color:var(--text-muted); font-family:'Outfit',sans-serif; }
  .dot { width:6px; height:6px; background:var(--success); border-radius:50%; animation:pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }

  main { position:relative; z-index:1; max-width:980px; margin:0 auto; padding:52px 24px 72px; }

  .hero { text-align:center; margin-bottom:52px; animation:up .65s ease both; }
  @keyframes up { from{opacity:0;transform:translateY(22px)} to{opacity:1;transform:translateY(0)} }

  .hero-chip {
    display:inline-flex; align-items:center; gap:7px;
    padding:6px 16px;
    background:rgba(59,130,246,0.08);
    border:1px solid rgba(59,130,246,0.2);
    border-radius:20px; margin-bottom:22px;
    font-size:0.78rem; color:var(--accent3);
    font-family:'Outfit',sans-serif; letter-spacing:.5px;
  }
  .hero h1 { font-size:clamp(2rem,5vw,3.2rem); font-weight:900; line-height:1.15; letter-spacing:-1px; margin-bottom:14px; }
  .hero h1 .grad {
    font-family:'Outfit',sans-serif;
    background:linear-gradient(90deg,var(--accent2),var(--accent3));
    -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text;
  }
  .hero p { color:var(--text-dim); font-size:1rem; font-weight:300; }

  /* Lang bar */
  .lang-bar {
    display:flex; align-items:center; gap:8px;
    background:var(--surface); border:1px solid var(--border);
    border-radius:var(--r); padding:8px; margin-bottom:14px;
    animation:up .65s .1s ease both;
  }


  .lang-select-wrap { flex:1; }
  .lang-select {
    width:100%; padding:11px 14px 11px 28px; border-radius:10px;
    background:var(--surface2); border:1px solid var(--border);
    color:var(--text); font-family:'Cairo',sans-serif; font-size:.85rem;
    font-weight:600; cursor:pointer; outline:none; transition:all .2s;
    -webkit-appearance:none; appearance:none;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='10' viewBox='0 0 10 10'%3E%3Cpath fill='%2364748b' d='M5 7L0 2h10z'/%3E%3C/svg%3E");
    background-repeat:no-repeat; background-position:8px center;
  }
  .lang-select:focus, .lang-select:hover { border-color:rgba(59,130,246,.45); color:var(--accent2); }
  .lang-select option { background:#111827; color:var(--text); }

  .swap-btn {
    width:44px; height:44px; border-radius:10px;
    background:var(--surface2); border:1px solid var(--border);
    color:var(--text-muted); cursor:pointer;
    display:flex; align-items:center; justify-content:center;
    font-size:1.2rem; transition:all .25s; flex-shrink:0;
  }
  .swap-btn:hover { background:var(--glow); border-color:var(--accent); color:var(--accent2); transform:rotate(180deg); }

  /* Panels */
  .translator { display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-bottom:14px; animation:up .65s .15s ease both; }
  @media(max-width:640px) { .translator{grid-template-columns:1fr} header{padding:0 16px} main{padding:32px 14px 52px} }

  .panel { background:var(--surface); border:1px solid var(--border); border-radius:var(--r); overflow:hidden; display:flex; flex-direction:column; transition:border-color .2s, box-shadow .2s; }
  .panel:focus-within { border-color:rgba(59,130,246,.4); box-shadow:0 0 0 3px rgba(59,130,246,.08); }

  .panel-head { display:flex; align-items:center; justify-content:space-between; padding:10px 14px; background:var(--surface2); border-bottom:1px solid var(--border); }
  .panel-lang { font-size:.78rem; font-weight:700; color:var(--text-muted); letter-spacing:1px; text-transform:uppercase; font-family:'Outfit',sans-serif; }

  .panel-btns { display:flex; gap:4px; }
  .pb { width:28px; height:28px; border-radius:7px; background:transparent; border:1px solid transparent; color:var(--text-muted); cursor:pointer; display:flex; align-items:center; justify-content:center; font-size:.85rem; transition:all .15s; }
  .pb:hover { background:var(--border); color:var(--text); }
  .pb.ok { color:var(--success) !important; background:rgba(16,185,129,.1) !important; }

  textarea { width:100%; flex:1; min-height:210px; background:transparent; border:none; outline:none; color:var(--text); font-family:'Cairo','Outfit',sans-serif; font-size:1rem; line-height:1.85; padding:16px; resize:none; }
  textarea::placeholder { color:var(--text-muted); }

  .out-body { flex:1; min-height:210px; padding:16px; font-size:1rem; line-height:1.85; color:var(--text); white-space:pre-wrap; word-break:break-word; }
  .ph { color:var(--text-muted); font-weight:300; }

  .cursor { display:inline-block; width:2px; height:1.1em; background:var(--accent2); margin-left:2px; vertical-align:middle; border-radius:1px; animation:blink 1s step-end infinite; }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

  .panel-foot { padding:8px 14px; border-top:1px solid var(--border); font-size:.72rem; color:var(--text-muted); font-family:'Outfit',sans-serif; }

  /* Translate btn */
  .btn-wrap { animation:up .65s .2s ease both; }
  .translate-btn {
    width:100%; padding:17px;
    background:linear-gradient(135deg,var(--accent) 0%,#6366f1 100%);
    border:none; border-radius:var(--r); color:#fff;
    font-family:'Cairo',sans-serif; font-size:1.1rem; font-weight:700;
    cursor:pointer; transition:all .22s;
    display:flex; align-items:center; justify-content:center; gap:10px;
    box-shadow:0 4px 22px rgba(59,130,246,.22);
    position:relative; overflow:hidden;
  }
  .translate-btn::after { content:''; position:absolute; inset:0; background:linear-gradient(135deg,rgba(255,255,255,.09),transparent); opacity:0; transition:opacity .2s; }
  .translate-btn:hover { transform:translateY(-2px); box-shadow:0 10px 34px rgba(59,130,246,.42); }
  .translate-btn:hover::after { opacity:1; }
  .translate-btn:active { transform:translateY(0); }
  .translate-btn:disabled { opacity:.45; cursor:not-allowed; transform:none; box-shadow:none; }

  .spinner { width:20px; height:20px; border:2px solid rgba(255,255,255,.3); border-top-color:#fff; border-radius:50%; animation:spin .65s linear infinite; }
  @keyframes spin { to{transform:rotate(360deg)} }

  /* Keyboard hint */
  .kb-hint { text-align:center; margin-top:10px; font-size:.73rem; color:var(--text-muted); font-family:'Outfit',sans-serif; }
  kbd { background:var(--surface2); border:1px solid var(--border); border-radius:5px; padding:2px 6px; font-size:.68rem; }

  /* Toast */
  .toast { position:fixed; bottom:32px; left:50%; transform:translateX(-50%) translateY(16px); background:#1e293b; border:1px solid #ef4444; color:#fca5a5; padding:12px 22px; border-radius:12px; font-size:.88rem; opacity:0; transition:all .3s; z-index:999; font-family:'Cairo',sans-serif; pointer-events:none; }
  .toast.show { opacity:1; transform:translateX(-50%) translateY(0); }

  footer { position:relative; z-index:1; text-align:center; padding:22px; font-size:.73rem; color:var(--text-muted); font-family:'Outfit',sans-serif; border-top:1px solid var(--border); }
</style>
</head>
<body>
<div class="orb orb1"></div>
<div class="orb orb2"></div>

<header>
  <div class="logo">
    <div class="logo-mark">🌐</div>
    <div>
      <div class="logo-text">Bo<em>da</em></div>
      <div class="logo-sub">Smart Translator · مترجم ذكي</div>
    </div>
  </div>
  <div class="badge"><div class="dot"></div>AI-Powered</div>
</header>

<main>
  <div class="hero">
    <div class="hero-chip">✦ مترجم ذكي · Smart Translator</div>
    <h1>
      <span class="grad">Boda</span> — ترجم بسهولة
    </h1>
    <p>ترجمة احترافية بين أكثر من 30 لغة — بدقة ووضوح</p>
  </div>

  <div class="lang-bar">
    <div class="lang-select-wrap">
      <select id="srcLang" onchange="onLangChange()" class="lang-select">
        <option value="Arabic">🇸🇦 العربية — Arabic</option>
        <option value="Franco Arabic">🔤 فرانكو — Franco Arabic</option>
        <option value="English" selected>🇬🇧 English — الإنجليزية</option>
        <option value="French">🇫🇷 Français — الفرنسية</option>
        <option value="Spanish">🇪🇸 Español — الإسبانية</option>
        <option value="German">🇩🇪 Deutsch — الألمانية</option>
        <option value="Italian">🇮🇹 Italiano — الإيطالية</option>
        <option value="Portuguese">🇵🇹 Português — البرتغالية</option>
        <option value="Russian">🇷🇺 Русский — الروسية</option>
        <option value="Chinese">🇨🇳 中文 — الصينية</option>
        <option value="Japanese">🇯🇵 日本語 — اليابانية</option>
        <option value="Korean">🇰🇷 한국어 — الكورية</option>
        <option value="Turkish">🇹🇷 Türkçe — التركية</option>
        <option value="Persian">🇮🇷 فارسی — الفارسية</option>
        <option value="Urdu">🇵🇰 اردو — الأردية</option>
        <option value="Hindi">🇮🇳 हिन्दी — الهندية</option>
        <option value="Bengali">🇧🇩 বাংলা — البنغالية</option>
        <option value="Dutch">🇳🇱 Nederlands — الهولندية</option>
        <option value="Polish">🇵🇱 Polski — البولندية</option>
        <option value="Swedish">🇸🇪 Svenska — السويدية</option>
        <option value="Greek">🇬🇷 Ελληνικά — اليونانية</option>
        <option value="Hebrew">🇮🇱 עברית — العبرية</option>
        <option value="Indonesian">🇮🇩 Bahasa Indonesia — الإندونيسية</option>
        <option value="Malay">🇲🇾 Bahasa Melayu — الملايو</option>
        <option value="Thai">🇹🇭 ภาษาไทย — التايلاندية</option>
        <option value="Vietnamese">🇻🇳 Tiếng Việt — الفيتنامية</option>
        <option value="Czech">🇨🇿 Čeština — التشيكية</option>
        <option value="Romanian">🇷🇴 Română — الرومانية</option>
        <option value="Hungarian">🇭🇺 Magyar — الهنغارية</option>
        <option value="Ukrainian">🇺🇦 Українська — الأوكرانية</option>
        <option value="Swahili">🇰🇪 Kiswahili — السواحيلية</option>
      </select>
    </div>
    <button class="swap-btn" onclick="swapLangs()" title="تبديل اللغتين">⇄</button>
    <div class="lang-select-wrap">
      <select id="tgtLang" onchange="onLangChange()" class="lang-select">
        <option value="Arabic" selected>🇸🇦 العربية — Arabic</option>
        <option value="Franco Arabic">🔤 فرانكو — Franco Arabic</option>
        <option value="English">🇬🇧 English — الإنجليزية</option>
        <option value="French">🇫🇷 Français — الفرنسية</option>
        <option value="Spanish">🇪🇸 Español — الإسبانية</option>
        <option value="German">🇩🇪 Deutsch — الألمانية</option>
        <option value="Italian">🇮🇹 Italiano — الإيطالية</option>
        <option value="Portuguese">🇵🇹 Português — البرتغالية</option>
        <option value="Russian">🇷🇺 Русский — الروسية</option>
        <option value="Chinese">🇨🇳 中文 — الصينية</option>
        <option value="Japanese">🇯🇵 日本語 — اليابانية</option>
        <option value="Korean">🇰🇷 한국어 — الكورية</option>
        <option value="Turkish">🇹🇷 Türkçe — التركية</option>
        <option value="Persian">🇮🇷 فارسی — الفارسية</option>
        <option value="Urdu">🇵🇰 اردو — الأردية</option>
        <option value="Hindi">🇮🇳 हिन्दी — الهندية</option>
        <option value="Bengali">🇧🇩 বাংলা — البنغالية</option>
        <option value="Dutch">🇳🇱 Nederlands — الهولندية</option>
        <option value="Polish">🇵🇱 Polski — البولندية</option>
        <option value="Swedish">🇸🇪 Svenska — السويدية</option>
        <option value="Greek">🇬🇷 Ελληνικά — اليونانية</option>
        <option value="Hebrew">🇮🇱 עברית — العبرية</option>
        <option value="Indonesian">🇮🇩 Bahasa Indonesia — الإندونيسية</option>
        <option value="Malay">🇲🇾 Bahasa Melayu — الملايو</option>
        <option value="Thai">🇹🇭 ภาษาไทย — التايلاندية</option>
        <option value="Vietnamese">🇻🇳 Tiếng Việt — الفيتنامية</option>
        <option value="Czech">🇨🇿 Čeština — التشيكية</option>
        <option value="Romanian">🇷🇴 Română — الرومانية</option>
        <option value="Hungarian">🇭🇺 Magyar — الهنغارية</option>
        <option value="Ukrainian">🇺🇦 Українська — الأوكرانية</option>
        <option value="Swahili">🇰🇪 Kiswahili — السواحيلية</option>
      </select>
    </div>
  </div>

  <div class="translator">
    <div class="panel">
      <div class="panel-head">
        <span class="panel-lang" id="inLabel">العربية</span>
        <div class="panel-btns">
          <button class="pb" onclick="clearAll()" title="مسح">✕</button>
          <button class="pb" id="cpIn" onclick="doCopy('in')" title="نسخ">⧉</button>
        </div>
      </div>
      <textarea id="inTxt" placeholder="اكتب النص هنا…" oninput="countChars()" dir="rtl"></textarea>
      <div class="panel-foot"><span id="cc">0 حرف</span></div>
    </div>

    <div class="panel">
      <div class="panel-head">
        <span class="panel-lang" id="outLabel">English</span>
        <div class="panel-btns">
          <button class="pb" id="cpOut" onclick="doCopy('out')" title="نسخ">⧉</button>
        </div>
      </div>
      <div class="out-body" id="outTxt" dir="ltr"><span class="ph">ستظهر الترجمة هنا…</span></div>
      <div class="panel-foot" id="outFoot"></div>
    </div>
  </div>

  <div class="btn-wrap">
    <button class="translate-btn" id="trBtn" onclick="doTranslate()">
      <span id="btnInner">🌐 &nbsp; ترجم الآن</span>
    </button>
    <div class="kb-hint"><kbd>Ctrl</kbd> + <kbd>Enter</kbd> للترجمة السريعة</div>
  </div>
</main>

<footer>Boda · مدعوم بذكاء اصطناعي · Powered by Claude AI</footer>
<div class="toast" id="toast"></div>

<script>
  let busy = false;

  // RTL languages
  const RTL_LANGS = new Set(['Arabic','Persian','Urdu','Hebrew']);

  function getLangDir(lang) { return RTL_LANGS.has(lang) ? 'rtl' : 'ltr'; }

  function onLangChange() {
    const src = document.getElementById('srcLang').value;
    const tgt = document.getElementById('tgtLang').value;
    const inp = document.getElementById('inTxt');
    inp.setAttribute('dir', getLangDir(src));
    document.getElementById('outTxt').setAttribute('dir', getLangDir(tgt));
    document.getElementById('inLabel').textContent = src;
    document.getElementById('outLabel').textContent = tgt;

    const placeholders = {
      'Arabic': 'اكتب النص هنا…',
      'Franco Arabic': 'Ekteb el nas hena... مثال: ana mabsout',
      'Persian': 'متن خود را اینجا بنویسید…',
      'Urdu': 'یہاں متن لکھیں…',
      'Hebrew': 'כתוב את הטקסט כאן…',
    };
    inp.placeholder = placeholders[src] || `Type your ${src} text here…`;
  }

  function swapLangs() {
    const srcSel = document.getElementById('srcLang');
    const tgtSel = document.getElementById('tgtLang');
    const prevSrc = srcSel.value;
    const prevTgt = tgtSel.value;
    const out = document.getElementById('outTxt');
    const inp = document.getElementById('inTxt');
    const prevResult = out.dataset.res || '';

    srcSel.value = prevTgt;
    tgtSel.value = prevSrc;
    onLangChange();

    if (prevResult) {
      inp.value = prevResult;
      out.innerHTML = '<span class="ph">ستظهر الترجمة هنا…</span>';
      out.dataset.res = '';
      document.getElementById('outFoot').textContent = '';
    }
    countChars();
  }

  function countChars() {
    const v = document.getElementById('inTxt').value;
    document.getElementById('cc').textContent = `${v.length} ${RTL_LANGS.has(document.getElementById('srcLang').value) ? 'حرف' : 'chars'}`;
  }

  function clearAll() {
    document.getElementById('inTxt').value = '';
    const out = document.getElementById('outTxt');
    out.innerHTML = '<span class="ph">ستظهر الترجمة هنا…</span>';
    out.dataset.res = '';
    document.getElementById('outFoot').textContent = '';
    countChars();
  }

  async function doCopy(which) {
    const text = which === 'in'
      ? document.getElementById('inTxt').value
      : (document.getElementById('outTxt').dataset.res || '');
    if (!text) { showToast('لا يوجد نص'); return; }
    const btnId = which === 'in' ? 'cpIn' : 'cpOut';
    try {
      await navigator.clipboard.writeText(text);
      const b = document.getElementById(btnId);
      b.textContent = '✓'; b.classList.add('ok');
      setTimeout(() => { b.textContent = '⧉'; b.classList.remove('ok'); }, 1600);
    } catch { showToast('فشل النسخ'); }
  }

  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg; t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 2800);
  }

  async function doTranslate() {
    if (busy) return;
    const text = document.getElementById('inTxt').value.trim();
    const srcLang = document.getElementById('srcLang').value;
    const tgtLang = document.getElementById('tgtLang').value;
    if (!text) { showToast('من فضلك اكتب نصاً أولاً'); return; }
    if (srcLang === tgtLang) { showToast('اختر لغتين مختلفتين'); return; }

    busy = true;
    const btn = document.getElementById('trBtn');
    btn.disabled = true;
    document.getElementById('btnInner').innerHTML = '<div class="spinner"></div>';

    const out = document.getElementById('outTxt');
    out.innerHTML = '<span class="cursor"></span>';
    out.setAttribute('dir', getLangDir(tgtLang));
    out.dataset.res = '';
    document.getElementById('outFoot').textContent = '';

    let prompt;
    if (srcLang === 'Franco Arabic') {
      prompt = `You are an expert in Egyptian/Arab Franco Arabic (Arabizi) — the romanized Arabic writing style used in chats (e.g., "ana mabsout", "kefak", "7abibi", "3andi", "2olt").

Translate the following Franco Arabic text into ${tgtLang}.

Instructions:
- Return ONLY the translation, nothing else
- Understand Franco Arabic conventions: 3=ع, 2=ء/أ, 7=ح, 5=خ, 6=ط, 8=ق, 9=ص
- Translate naturally and fluently

Franco Arabic text:
${text}`;
    } else if (tgtLang === 'Franco Arabic') {
      prompt = `You are an expert in Egyptian/Arab Franco Arabic (Arabizi) — the romanized Arabic writing style used in chats (e.g., "ana mabsout", "kefak", "7abibi", "3andi", "2olt").

Translate the following ${srcLang} text into Franco Arabic (Arabizi).

Instructions:
- Return ONLY the Franco Arabic transliteration/translation, nothing else
- Use common Franco Arabic conventions: 3=ع, 2=ء/أ, 7=ح, 5=خ, 6=ط, 8=ق, 9=ص
- Write in a natural, conversational Franco Arabic style as used in Egyptian/Arab social media

Text:
${text}`;
    } else {
      prompt = `You are a professional translator. Translate the following ${srcLang} text into ${tgtLang}.

Instructions:
- Return ONLY the translation, nothing else — no explanations, no notes, no preamble
- Preserve line breaks, punctuation, and formatting
- Use natural, fluent, idiomatic language appropriate for native speakers
- If it is a headline or title, maintain that style

Text to translate:
${text}`;
    }

    try {
      const res = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 2000,
          messages: [{ role: 'user', content: prompt }]
        })
      });

      const data = await res.json();
      if (!res.ok) throw new Error(data.error?.message || 'API Error');

      const result = (data.content?.[0]?.text || '').trim();
      out.dataset.res = result;

      // Typewriter effect
      out.textContent = '';
      const spd = Math.max(5, Math.min(20, 1600 / result.length));
      let i = 0;
      const cur = document.createElement('span');
      cur.className = 'cursor';

      function type() {
        if (i < result.length) {
          out.textContent = result.slice(0, ++i);
          out.appendChild(cur);
          setTimeout(type, spd);
        } else {
          out.textContent = result;
          document.getElementById('outFoot').textContent = `${result.length} ${RTL_LANGS.has(tgtLang) ? 'حرف' : 'chars'}`;
        }
      }
      type();

    } catch (e) {
      out.innerHTML = '<span class="ph">حدث خطأ، حاول مرة أخرى</span>';
      showToast('⚠️ خطأ في الاتصال');
      console.error(e);
    } finally {
      busy = false;
      btn.disabled = false;
      document.getElementById('btnInner').innerHTML = '🌐 &nbsp; ترجم الآن';
    }
  }

  document.addEventListener('keydown', e => {
    if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') doTranslate();
  });

  // Init
  onLangChange();
</script>
</body>
</html>
