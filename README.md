import { useState, useCallback } from "react";

const LANGUAGES = [
  { value: "Arabic", label: "🇸🇦 العربية", sub: "Arabic", rtl: true },
  { value: "Franco Arabic", label: "🔤 فرانكو", sub: "Franco Arabic", rtl: false },
  { value: "English", label: "🇬🇧 English", sub: "الإنجليزية", rtl: false },
  { value: "French", label: "🇫🇷 Français", sub: "الفرنسية", rtl: false },
  { value: "Spanish", label: "🇪🇸 Español", sub: "الإسبانية", rtl: false },
  { value: "German", label: "🇩🇪 Deutsch", sub: "الألمانية", rtl: false },
  { value: "Italian", label: "🇮🇹 Italiano", sub: "الإيطالية", rtl: false },
  { value: "Portuguese", label: "🇵🇹 Português", sub: "البرتغالية", rtl: false },
  { value: "Russian", label: "🇷🇺 Русский", sub: "الروسية", rtl: false },
  { value: "Chinese", label: "🇨🇳 中文", sub: "الصينية", rtl: false },
  { value: "Japanese", label: "🇯🇵 日本語", sub: "اليابانية", rtl: false },
  { value: "Korean", label: "🇰🇷 한국어", sub: "الكورية", rtl: false },
  { value: "Turkish", label: "🇹🇷 Türkçe", sub: "التركية", rtl: false },
  { value: "Persian", label: "🇮🇷 فارسی", sub: "الفارسية", rtl: true },
  { value: "Urdu", label: "🇵🇰 اردو", sub: "الأردية", rtl: true },
  { value: "Hindi", label: "🇮🇳 हिन्दी", sub: "الهندية", rtl: false },
  { value: "Bengali", label: "🇧🇩 বাংলা", sub: "البنغالية", rtl: false },
  { value: "Dutch", label: "🇳🇱 Nederlands", sub: "الهولندية", rtl: false },
  { value: "Polish", label: "🇵🇱 Polski", sub: "البولندية", rtl: false },
  { value: "Swedish", label: "🇸🇪 Svenska", sub: "السويدية", rtl: false },
  { value: "Greek", label: "🇬🇷 Ελληνικά", sub: "اليونانية", rtl: false },
  { value: "Hebrew", label: "🇮🇱 עברית", sub: "العبرية", rtl: true },
  { value: "Indonesian", label: "🇮🇩 Indonesia", sub: "الإندونيسية", rtl: false },
  { value: "Thai", label: "🇹🇭 ภาษาไทย", sub: "التايلاندية", rtl: false },
  { value: "Vietnamese", label: "🇻🇳 Tiếng Việt", sub: "الفيتنامية", rtl: false },
  { value: "Ukrainian", label: "🇺🇦 Українська", sub: "الأوكرانية", rtl: false },
  { value: "Swahili", label: "🇰🇪 Kiswahili", sub: "السواحيلية", rtl: false },
];

function buildPrompt(text, srcLang, tgtLang) {
  if (srcLang === "Franco Arabic") {
    return `You are an expert in Egyptian/Arab Franco Arabic (Arabizi). Translate into ${tgtLang}. Rules: ONLY return the translation, nothing else. Understand: 3=ع, 2=أ, 7=ح, 5=خ.\n\nText:\n${text}`;
  }
  if (tgtLang === "Franco Arabic") {
    return `You are an expert in Egyptian Franco Arabic (Arabizi). Translate the following ${srcLang} into Franco Arabic. Rules: ONLY return Franco Arabic, nothing else. Use: 3=ع, 2=أ, 7=ح, 5=خ.\n\nText:\n${text}`;
  }
  return `Translate the following ${srcLang} text into ${tgtLang}. Return ONLY the translation, no explanations.\n\nText:\n${text}`;
}

export default function BodaTranslator() {
  const [srcLang, setSrcLang] = useState("Arabic");
  const [tgtLang, setTgtLang] = useState("English");
  const [inputText, setInputText] = useState("");
  const [outputText, setOutputText] = useState("");
  const [loading, setLoading] = useState(false);
  const [toast, setToast] = useState("");
  const [copiedIn, setCopiedIn] = useState(false);
  const [copiedOut, setCopiedOut] = useState(false);

  const srcInfo = LANGUAGES.find(l => l.value === srcLang) || LANGUAGES[0];
  const tgtInfo = LANGUAGES.find(l => l.value === tgtLang) || LANGUAGES[2];

  const showToast = (msg) => {
    setToast(msg);
    setTimeout(() => setToast(""), 2800);
  };

  const swap = () => {
    const prevSrc = srcLang, prevTgt = tgtLang, prevOut = outputText;
    setSrcLang(prevTgt);
    setTgtLang(prevSrc);
    if (prevOut) { setInputText(prevOut); setOutputText(""); }
  };

  const copy = async (which) => {
    const text = which === "in" ? inputText : outputText;
    if (!text) { showToast("لا يوجد نص"); return; }
    try {
      await navigator.clipboard.writeText(text);
      if (which === "in") { setCopiedIn(true); setTimeout(() => setCopiedIn(false), 1500); }
      else { setCopiedOut(true); setTimeout(() => setCopiedOut(false), 1500); }
    } catch { showToast("فشل النسخ"); }
  };

  const translate = useCallback(async () => {
    if (loading) return;
    const text = inputText.trim();
    if (!text) { showToast("اكتب نصاً أولاً"); return; }
    if (srcLang === tgtLang) { showToast("اختر لغتين مختلفتين"); return; }

    setLoading(true);
    setOutputText("");

    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 2000,
          messages: [{ role: "user", content: buildPrompt(text, srcLang, tgtLang) }]
        })
      });
      const data = await res.json();
      if (!res.ok) throw new Error(data.error?.message || "Error");
      const result = (data.content?.[0]?.text || "").trim();
      setOutputText(result);
    } catch (e) {
      showToast("⚠️ خطأ، حاول مرة أخرى");
      console.error(e);
    } finally {
      setLoading(false);
    }
  }, [loading, inputText, srcLang, tgtLang]);

  const s = {
    wrap: { minHeight: "100vh", background: "#080b11", color: "#e2e8f0", fontFamily: "'Cairo', sans-serif", position: "relative", overflow: "hidden" },
    orb1: { position: "fixed", width: 700, height: 700, borderRadius: "50%", background: "radial-gradient(circle,rgba(59,130,246,0.07) 0%,transparent 70%)", top: -200, right: -150, filter: "blur(120px)", pointerEvents: "none", zIndex: 0 },
    orb2: { position: "fixed", width: 500, height: 500, borderRadius: "50%", background: "radial-gradient(circle,rgba(99,102,241,0.05) 0%,transparent 70%)", bottom: -100, left: -100, filter: "blur(100px)", pointerEvents: "none", zIndex: 0 },
    header: { position: "sticky", top: 0, zIndex: 100, background: "rgba(8,11,17,0.9)", backdropFilter: "blur(18px)", borderBottom: "1px solid #1a2540", padding: "0 32px", height: 64, display: "flex", alignItems: "center", justifyContent: "space-between" },
    logoWrap: { display: "flex", alignItems: "center", gap: 12 },
    logoMark: { width: 40, height: 40, background: "linear-gradient(135deg,#3b82f6,#6366f1)", borderRadius: 11, display: "flex", alignItems: "center", justifyContent: "center", fontSize: "1.2rem", boxShadow: "0 4px 18px rgba(59,130,246,0.35)" },
    logoText: { fontSize: "1.5rem", fontWeight: 900, fontFamily: "'Outfit', sans-serif", letterSpacing: -1 },
    logoSub: { fontSize: "0.66rem", color: "#4a5a72", letterSpacing: 2, textTransform: "uppercase", fontFamily: "'Outfit', sans-serif" },
    badge: { display: "flex", alignItems: "center", gap: 7, padding: "6px 13px", background: "#0f1521", border: "1px solid #1a2540", borderRadius: 20, fontSize: "0.72rem", color: "#4a5a72", fontFamily: "'Outfit', sans-serif" },
    dot: { width: 6, height: 6, background: "#10b981", borderRadius: "50%" },
    main: { position: "relative", zIndex: 1, maxWidth: 960, margin: "0 auto", padding: "48px 20px 64px" },
    hero: { textAlign: "center", marginBottom: 44 },
    chip: { display: "inline-flex", alignItems: "center", gap: 6, padding: "5px 14px", background: "rgba(59,130,246,0.08)", border: "1px solid rgba(59,130,246,0.2)", borderRadius: 20, marginBottom: 18, fontSize: "0.76rem", color: "#93c5fd", fontFamily: "'Outfit',sans-serif", letterSpacing: 0.5 },
    h1: { fontSize: "clamp(1.9rem,4.5vw,3rem)", fontWeight: 900, lineHeight: 1.15, letterSpacing: -1, marginBottom: 12 },
    grad: { background: "linear-gradient(90deg,#60a5fa,#93c5fd)", WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent", backgroundClip: "text", fontFamily: "'Outfit',sans-serif" },
    heroP: { color: "#94a3b8", fontSize: "0.95rem", fontWeight: 300 },
    langBar: { display: "flex", alignItems: "center", gap: 8, background: "#0f1521", border: "1px solid #1a2540", borderRadius: 14, padding: 8, marginBottom: 12 },
    select: { flex: 1, padding: "11px 12px", borderRadius: 10, background: "#151d2e", border: "1px solid #1a2540", color: "#e2e8f0", fontFamily: "'Cairo',sans-serif", fontSize: "0.87rem", fontWeight: 600, cursor: "pointer", outline: "none" },
    swapBtn: { width: 44, height: 44, borderRadius: 10, background: "#151d2e", border: "1px solid #1a2540", color: "#4a5a72", cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "center", fontSize: "1.2rem", flexShrink: 0, transition: "all .2s" },
    grid: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: 14, marginBottom: 14 },
    panel: { background: "#0f1521", border: "1px solid #1a2540", borderRadius: 14, overflow: "hidden", display: "flex", flexDirection: "column" },
    panelHead: { display: "flex", alignItems: "center", justifyContent: "space-between", padding: "10px 14px", background: "#151d2e", borderBottom: "1px solid #1a2540" },
    panelLang: { fontSize: "0.76rem", fontWeight: 700, color: "#4a5a72", letterSpacing: 1, textTransform: "uppercase", fontFamily: "'Outfit',sans-serif" },
    panelBtns: { display: "flex", gap: 4 },
    pb: (active) => ({ width: 28, height: 28, borderRadius: 7, background: active ? "rgba(16,185,129,0.1)" : "transparent", border: "1px solid transparent", color: active ? "#10b981" : "#4a5a72", cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "center", fontSize: "0.82rem" }),
    textarea: (rtl) => ({ width: "100%", flex: 1, minHeight: 210, background: "transparent", border: "none", outline: "none", color: "#e2e8f0", fontFamily: "'Cairo','Outfit',sans-serif", fontSize: "1rem", lineHeight: 1.85, padding: 16, resize: "none", direction: rtl ? "rtl" : "ltr" }),
    outBody: (rtl) => ({ flex: 1, minHeight: 210, padding: 16, fontSize: "1rem", lineHeight: 1.85, color: "#e2e8f0", whiteSpace: "pre-wrap", wordBreak: "break-word", direction: rtl ? "rtl" : "ltr" }),
    outPh: { color: "#4a5a72", fontWeight: 300 },
    panelFoot: { padding: "8px 14px", borderTop: "1px solid #1a2540", fontSize: "0.71rem", color: "#4a5a72", fontFamily: "'Outfit',sans-serif" },
    translateBtn: { width: "100%", padding: 17, background: loading ? "rgba(59,130,246,0.5)" : "linear-gradient(135deg,#3b82f6,#6366f1)", border: "none", borderRadius: 14, color: "#fff", fontFamily: "'Cairo',sans-serif", fontSize: "1.1rem", fontWeight: 700, cursor: loading ? "not-allowed" : "pointer", display: "flex", alignItems: "center", justifyContent: "center", gap: 10, boxShadow: "0 4px 22px rgba(59,130,246,0.22)" },
    kbHint: { textAlign: "center", marginTop: 10, fontSize: "0.71rem", color: "#4a5a72", fontFamily: "'Outfit',sans-serif" },
    kbd: { background: "#151d2e", border: "1px solid #1a2540", borderRadius: 5, padding: "2px 6px", fontSize: "0.65rem" },
    toast: { position: "fixed", bottom: 32, left: "50%", transform: "translateX(-50%)", background: "#1e293b", border: "1px solid #ef4444", color: "#fca5a5", padding: "12px 22px", borderRadius: 12, fontSize: "0.88rem", fontFamily: "'Cairo',sans-serif", zIndex: 999, pointerEvents: "none", opacity: toast ? 1 : 0, transition: "opacity .3s" },
    footer: { position: "relative", zIndex: 1, textAlign: "center", padding: 22, fontSize: "0.71rem", color: "#4a5a72", fontFamily: "'Outfit',sans-serif", borderTop: "1px solid #1a2540" },
  };

  return (
    <div style={s.wrap} dir="rtl">
      <div style={s.orb1} />
      <div style={s.orb2} />

      {/* Header */}
      <header style={s.header}>
        <div style={s.logoWrap}>
          <div style={s.logoMark}>🌐</div>
          <div>
            <div style={s.logoText}>Bo<span style={{ color: "#60a5fa" }}>da</span></div>
            <div style={s.logoSub}>Smart Translator · مترجم ذكي</div>
          </div>
        </div>
        <div style={s.badge}><div style={s.dot} />AI-Powered</div>
      </header>

      <main style={s.main}>
        {/* Hero */}
        <div style={s.hero}>
          <div style={s.chip}>✦ مترجم ذكي · Smart Translator</div>
          <h1 style={s.h1}>
            <span style={s.grad}>Boda</span> — ترجم بسهولة
          </h1>
          <p style={s.heroP}>ترجمة احترافية بين أكثر من 30 لغة — بدقة ووضوح</p>
        </div>

        {/* Language Bar */}
        <div style={s.langBar}>
          <select style={s.select} value={srcLang} onChange={e => setSrcLang(e.target.value)}>
            {LANGUAGES.map(l => <option key={l.value} value={l.value}>{l.label} — {l.sub}</option>)}
          </select>
          <button style={s.swapBtn} onClick={swap} title="تبديل">⇄</button>
          <select style={s.select} value={tgtLang} onChange={e => setTgtLang(e.target.value)}>
            {LANGUAGES.map(l => <option key={l.value} value={l.value}>{l.label} — {l.sub}</option>)}
          </select>
        </div>

        {/* Panels */}
        <div style={s.grid}>
          {/* Input */}
          <div style={s.panel}>
            <div style={s.panelHead}>
              <span style={s.panelLang}>{srcLang}</span>
              <div style={s.panelBtns}>
                <button style={s.pb(false)} onClick={() => { setInputText(""); setOutputText(""); }} title="مسح">✕</button>
                <button style={s.pb(copiedIn)} onClick={() => copy("in")} title="نسخ">{copiedIn ? "✓" : "⧉"}</button>
              </div>
            </div>
            <textarea
              style={s.textarea(srcInfo.rtl)}
              value={inputText}
              onChange={e => setInputText(e.target.value)}
              placeholder={
                srcLang === "Arabic" ? "اكتب النص هنا…" :
                srcLang === "Franco Arabic" ? "Ekteb hena... e.g. ana mabsout awy" :
                srcLang === "Persian" ? "متن خود را اینجا بنویسید…" :
                srcLang === "Urdu" ? "یہاں متن لکھیں…" :
                srcLang === "Hebrew" ? "כתוב את הטקסט כאן…" :
                `Type your ${srcLang} text here…`
              }
              onKeyDown={e => { if ((e.ctrlKey || e.metaKey) && e.key === "Enter") translate(); }}
            />
            <div style={s.panelFoot}>{inputText.length} {srcInfo.rtl ? "حرف" : "chars"}</div>
          </div>

          {/* Output */}
          <div style={s.panel}>
            <div style={s.panelHead}>
              <span style={s.panelLang}>{tgtLang}</span>
              <div style={s.panelBtns}>
                <button style={s.pb(copiedOut)} onClick={() => copy("out")} title="نسخ">{copiedOut ? "✓" : "⧉"}</button>
              </div>
            </div>
            <div style={s.outBody(tgtInfo.rtl)}>
              {loading ? (
                <span style={{ color: "#60a5fa", fontWeight: 300 }}>جاري الترجمة…</span>
              ) : outputText ? (
                outputText
              ) : (
                <span style={s.outPh}>ستظهر الترجمة هنا…</span>
              )}
            </div>
            <div style={s.panelFoot}>{outputText ? `${outputText.length} ${tgtInfo.rtl ? "حرف" : "chars"}` : ""}</div>
          </div>
        </div>

        {/* Button */}
        <button style={s.translateBtn} onClick={translate} disabled={loading}>
          {loading ? (
            <span style={{ display: "flex", alignItems: "center", gap: 10 }}>
              <span style={{ width: 20, height: 20, border: "2px solid rgba(255,255,255,0.3)", borderTopColor: "#fff", borderRadius: "50%", display: "inline-block", animation: "spin 0.65s linear infinite" }} />
              جاري الترجمة…
            </span>
          ) : "🌐   ترجم الآن"}
        </button>
        <div style={s.kbHint}><kbd style={s.kbd}>Ctrl</kbd> + <kbd style={s.kbd}>Enter</kbd> للترجمة السريعة</div>
      </main>

      <footer style={s.footer}>Boda · مدعوم بذكاء اصطناعي · Powered by Claude AI</footer>
      {toast && <div style={s.toast}>{toast}</div>}

      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&family=Outfit:wght@300;400;600;700;900&display=swap');
        @keyframes spin { to { transform: rotate(360deg); } }
        select option { background: #111827; }
        * { box-sizing: border-box; }
        @media (max-width: 600px) {
          div[style*="gridTemplateColumns"] { grid-template-columns: 1fr !important; }
        }
      `}</style>
    </div>
  );
}
