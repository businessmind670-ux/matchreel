<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MatchReel — Football Clips</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:ital,wght@0,400;0,600;0,700;0,900;1,700;1,900&family=Inter:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');

  :root {
    --bg: #080C08;
    --surface: #111811;
    --card: #0E150E;
    --border: rgba(255,255,255,0.07);
    --lime: #C8F135;
    --lime-glow: rgba(200,241,53,0.18);
    --red: #E8303A;
    --amber: #F5A623;
    --blue: #4A90D9;
    --purple: #9B59B6;
    --chalk: #EFF4EF;
    --dim: rgba(239,244,239,0.4);
    --dimmer: rgba(239,244,239,0.18);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--chalk);
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── SCANLINE TEXTURE ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.03) 2px,
      rgba(0,0,0,0.03) 4px
    );
    pointer-events: none;
    z-index: 999;
  }

  /* ── NAV ── */
  nav {
    position: sticky;
    top: 0;
    z-index: 100;
    height: 58px;
    display: flex;
    align-items: center;
    padding: 0 24px;
    background: rgba(8,12,8,0.95);
    backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--border);
    gap: 24px;
  }

  .logo {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 20px;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: var(--chalk);
    flex-shrink: 0;
  }
  .logo em { color: var(--lime); font-style: normal; }

  .nav-search {
    flex: 1;
    max-width: 420px;
    position: relative;
  }
  .nav-search input {
    width: 100%;
    background: rgba(255,255,255,0.05);
    border: 1px solid var(--border);
    color: var(--chalk);
    padding: 8px 14px 8px 36px;
    border-radius: 5px;
    font-size: 13px;
    font-family: 'Inter', sans-serif;
    outline: none;
    transition: border-color 0.2s;
  }
  .nav-search input::placeholder { color: var(--dimmer); }
  .nav-search input:focus { border-color: rgba(200,241,53,0.4); }
  .nav-search-icon {
    position: absolute;
    left: 11px;
    top: 50%;
    transform: translateY(-50%);
    color: var(--dimmer);
    font-size: 14px;
    pointer-events: none;
  }

  .nav-count {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--dim);
    margin-left: auto;
    flex-shrink: 0;
  }
  .nav-count strong { color: var(--lime); }

  /* ── HERO ── */
  .hero {
    position: relative;
    padding: 64px 24px 48px;
    text-align: center;
    overflow: hidden;
  }

  /* Glowing pitch circle behind hero text */
  .hero::before {
    content: '';
    position: absolute;
    top: -60px;
    left: 50%;
    transform: translateX(-50%);
    width: 600px;
    height: 600px;
    background: radial-gradient(ellipse, rgba(200,241,53,0.06) 0%, transparent 65%);
    pointer-events: none;
  }

  .hero-tag {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--lime);
    text-transform: uppercase;
    letter-spacing: 0.14em;
    border: 1px solid rgba(200,241,53,0.25);
    padding: 5px 14px;
    border-radius: 20px;
    margin-bottom: 24px;
  }
  .live-pip {
    width: 6px; height: 6px;
    background: var(--lime);
    border-radius: 50%;
    animation: blink 1.8s infinite;
  }
  @keyframes blink {
    0%,100%{opacity:1} 50%{opacity:0.2}
  }

  .hero h1 {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: clamp(48px, 9vw, 96px);
    line-height: 0.9;
    text-transform: uppercase;
    letter-spacing: -0.01em;
    margin-bottom: 20px;
    position: relative;
  }
  .hero h1 .italic-line {
    font-style: italic;
    color: var(--lime);
    display: block;
  }

  .hero-sub {
    font-size: 15px;
    color: var(--dim);
    max-width: 380px;
    margin: 0 auto 32px;
    line-height: 1.7;
  }

  /* ── FILTER BAR ── */
  .filter-bar {
    display: flex;
    gap: 8px;
    padding: 0 24px 28px;
    overflow-x: auto;
    scrollbar-width: none;
    -webkit-overflow-scrolling: touch;
  }
  .filter-bar::-webkit-scrollbar { display: none; }

  .filter-pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 7px 16px;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--dim);
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
    white-space: nowrap;
    transition: all 0.18s;
    font-family: 'Inter', sans-serif;
  }
  .filter-pill:hover { border-color: rgba(200,241,53,0.3); color: var(--chalk); }
  .filter-pill.active {
    background: var(--lime);
    border-color: var(--lime);
    color: #080C08;
    font-weight: 600;
  }

  /* ── FEATURED CLIP ── */
  .featured-wrap {
    padding: 0 24px 40px;
    max-width: 1100px;
    margin: 0 auto;
  }

  .section-eyebrow {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    color: var(--lime);
    text-transform: uppercase;
    letter-spacing: 0.16em;
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-eyebrow::after { content: ''; flex:1; height:1px; background: var(--border); }

  .featured-card {
    background: var(--surface);
    border: 1px solid rgba(200,241,53,0.2);
    border-radius: 10px;
    overflow: hidden;
    display: grid;
    grid-template-columns: 1fr 320px;
  }

  .featured-video {
    aspect-ratio: 16/9;
    background: #000;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    overflow: hidden;
  }
  /* Pitch texture on featured */
  .featured-video::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
      linear-gradient(180deg, transparent 60%, rgba(0,0,0,0.7) 100%),
      repeating-linear-gradient(90deg,
        rgba(255,255,255,0.02) 0%, rgba(255,255,255,0.02) 8%,
        transparent 8%, transparent 9%
      );
  }

  .big-play {
    width: 72px; height: 72px;
    background: rgba(200,241,53,0.12);
    border: 2px solid var(--lime);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 26px;
    color: var(--lime);
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    z-index: 1;
  }
  .big-play:hover { background: rgba(200,241,53,0.22); transform: scale(1.06); }

  .featured-emoji {
    position: absolute;
    font-size: 80px;
    opacity: 0.08;
    pointer-events: none;
  }

  .video-corner-badge {
    position: absolute;
    top: 14px; left: 14px;
    z-index: 2;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    padding: 4px 10px;
    border-radius: 3px;
  }
  .badge-goal { background: var(--lime); color: #080C08; }
  .badge-save { background: var(--blue); color: #fff; }
  .badge-assist { background: var(--amber); color: #080C08; }
  .badge-tackle { background: var(--red); color: #fff; }
  .badge-skill { background: var(--purple); color: #fff; }
  .badge-free-kick { background: #E67E22; color: #fff; }
  .badge-header { background: #16A085; color: #fff; }

  .video-corner-time {
    position: absolute;
    bottom: 14px; right: 14px;
    z-index: 2;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    background: rgba(0,0,0,0.7);
    padding: 3px 10px;
    border-radius: 3px;
  }

  .featured-info {
    padding: 28px 24px;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .featured-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 800;
    font-size: 26px;
    text-transform: uppercase;
    line-height: 1.1;
    letter-spacing: 0.02em;
  }

  .meta-row {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .meta-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 12px;
    color: var(--dim);
  }
  .meta-icon { width: 16px; text-align: center; flex-shrink: 0; }

  .clip-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }
  .clip-tag {
    font-size: 11px;
    padding: 3px 10px;
    border: 1px solid var(--border);
    border-radius: 20px;
    color: var(--dimmer);
    font-family: 'JetBrains Mono', monospace;
  }

  .download-btn-big {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    padding: 14px 20px;
    background: var(--lime);
    color: #080C08;
    border: none;
    border-radius: 6px;
    font-size: 14px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    cursor: pointer;
    transition: all 0.18s;
    font-family: 'Inter', sans-serif;
    margin-top: auto;
  }
  .download-btn-big:hover { background: #d6ff3f; transform: translateY(-1px); }

  .download-options {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }
  .download-opt {
    padding: 9px 12px;
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    border-radius: 5px;
    text-align: center;
    cursor: pointer;
    font-size: 12px;
    font-weight: 500;
    color: var(--dim);
    transition: all 0.15s;
  }
  .download-opt:hover { border-color: rgba(200,241,53,0.3); color: var(--chalk); }
  .download-opt span { display: block; font-family: 'JetBrains Mono', monospace; font-size: 10px; color: var(--dimmer); margin-top: 2px; }

  /* ── CLIP GRID ── */
  .clips-wrap {
    padding: 0 24px 60px;
    max-width: 1100px;
    margin: 0 auto;
  }

  .clips-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 16px;
  }

  .clip-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.2s;
  }
  .clip-card:hover {
    border-color: rgba(200,241,53,0.3);
    transform: translateY(-3px);
    box-shadow: 0 8px 32px rgba(0,0,0,0.4);
  }

  .clip-thumb {
    position: relative;
    aspect-ratio: 16/9;
    background: #0a0f0a;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }
  .clip-thumb-emoji {
    font-size: 48px;
    opacity: 0.12;
    position: absolute;
  }
  .clip-thumb-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 40%, rgba(0,0,0,0.6) 100%);
  }
  .clip-play-hover {
    position: absolute;
    width: 48px; height: 48px;
    background: rgba(200,241,53,0.1);
    border: 1.5px solid rgba(200,241,53,0.5);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    color: var(--lime);
    opacity: 0;
    transition: opacity 0.2s;
    z-index: 2;
  }
  .clip-card:hover .clip-play-hover { opacity: 1; }

  .clip-duration {
    position: absolute;
    bottom: 8px; right: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    background: rgba(0,0,0,0.75);
    padding: 2px 8px;
    border-radius: 3px;
    z-index: 2;
  }

  .clip-body { padding: 14px 16px; }
  .clip-title {
    font-weight: 600;
    font-size: 14px;
    line-height: 1.35;
    margin-bottom: 8px;
  }
  .clip-bottom {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .clip-info-text { font-size: 11px; color: var(--dim); }
  .clip-info-text span { display: block; }

  .dl-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 7px 14px;
    background: rgba(200,241,53,0.1);
    border: 1px solid rgba(200,241,53,0.3);
    border-radius: 4px;
    color: var(--lime);
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.15s;
    text-transform: uppercase;
    letter-spacing: 0.04em;
    font-family: 'Inter', sans-serif;
  }
  .dl-btn:hover { background: rgba(200,241,53,0.18); }

  /* ── DOWNLOAD TOAST ── */
  .toast {
    position: fixed;
    bottom: 28px;
    left: 50%;
    transform: translateX(-50%) translateY(80px);
    background: var(--lime);
    color: #080C08;
    padding: 12px 24px;
    border-radius: 6px;
    font-weight: 700;
    font-size: 13px;
    z-index: 9999;
    transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    pointer-events: none;
    white-space: nowrap;
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  /* ── LIGHTBOX ── */
  .lightbox {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.92);
    z-index: 500;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s;
  }
  .lightbox.open { opacity: 1; pointer-events: all; }

  .lightbox-inner {
    background: var(--surface);
    border: 1px solid rgba(200,241,53,0.2);
    border-radius: 10px;
    overflow: hidden;
    max-width: 780px;
    width: 100%;
  }

  .lightbox-video {
    aspect-ratio: 16/9;
    background: #000;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 64px;
    position: relative;
  }
  .lightbox-video::before {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(90deg,
      rgba(255,255,255,0.015) 0%, rgba(255,255,255,0.015) 12%,
      transparent 12%, transparent 13%
    );
  }

  .lightbox-meta {
    padding: 20px 24px;
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 20px;
  }
  .lightbox-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 800;
    font-size: 22px;
    text-transform: uppercase;
    margin-bottom: 6px;
  }
  .lightbox-sub { font-size: 12px; color: var(--dim); }

  .lightbox-dl {
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex-shrink: 0;
  }
  .lightbox-dl-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 10px 18px;
    border-radius: 5px;
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.15s;
    font-family: 'Inter', sans-serif;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    border: none;
    white-space: nowrap;
  }
  .lightbox-dl-btn.primary { background: var(--lime); color: #080C08; }
  .lightbox-dl-btn.primary:hover { background: #d6ff3f; }
  .lightbox-dl-btn.secondary { background: rgba(255,255,255,0.06); color: var(--chalk); border: 1px solid var(--border); }
  .lightbox-dl-btn.secondary:hover { border-color: rgba(200,241,53,0.3); }

  .lightbox-close {
    position: absolute;
    top: 16px; right: 16px;
    width: 36px; height: 36px;
    background: rgba(255,255,255,0.08);
    border: none;
    border-radius: 50%;
    color: var(--chalk);
    font-size: 18px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.15s;
    z-index: 10;
  }
  .lightbox-close:hover { background: rgba(255,255,255,0.15); }
  .lightbox { position: fixed; }
  .lightbox-inner { position: relative; }

  /* ── STATS BAR ── */
  .stats-bar {
    display: flex;
    justify-content: center;
    gap: 0;
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    margin: 0 0 40px;
    overflow-x: auto;
  }
  .stats-bar::-webkit-scrollbar { display: none; }
  .stat-cell {
    flex: 1;
    min-width: 120px;
    padding: 20px 16px;
    text-align: center;
    border-right: 1px solid var(--border);
  }
  .stat-cell:last-child { border-right: none; }
  .stat-n {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 32px;
    color: var(--lime);
    line-height: 1;
  }
  .stat-l {
    font-size: 11px;
    color: var(--dim);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-top: 4px;
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 700px) {
    .featured-card { grid-template-columns: 1fr; }
    .featured-info { padding: 20px 16px; }
    nav { gap: 12px; }
    .nav-count { display: none; }
    .hero { padding: 40px 16px 28px; }
    .featured-wrap, .clips-wrap { padding-left: 16px; padding-right: 16px; }
    .filter-bar { padding-left: 16px; }
    .stats-bar { flex-wrap: wrap; }
    .stat-cell { min-width: 50%; }
  }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="logo">Match<em>Reel</em></div>
  <div class="nav-search">
    <span class="nav-search-icon">🔍</span>
    <input type="text" placeholder="Search clips, players, teams..." oninput="filterClips(this.value)">
  </div>
  <div class="nav-count">
    <strong id="clipCount">24</strong> clips available
  </div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-tag"><span class="live-pip"></span> Updated daily</div>
  <h1>
    Best Football
    <span class="italic-line">Moments</span>
  </h1>
  <p class="hero-sub">Watch and download top football clips — goals, saves, skills and more. Free, no sign-up needed.</p>
</section>

<!-- FILTER BAR -->
<div class="filter-bar" id="filterBar">
  <button class="filter-pill active" onclick="setFilter('all', this)">⚽ All Clips</button>
  <button class="filter-pill" onclick="setFilter('goal', this)">🥅 Goals</button>
  <button class="filter-pill" onclick="setFilter('save', this)">🧤 Saves</button>
  <button class="filter-pill" onclick="setFilter('assist', this)">🎯 Assists</button>
  <button class="filter-pill" onclick="setFilter('skill', this)">✨ Skills</button>
  <button class="filter-pill" onclick="setFilter('tackle', this)">🛡 Tackles</button>
  <button class="filter-pill" onclick="setFilter('free-kick', this)">🌀 Free Kicks</button>
  <button class="filter-pill" onclick="setFilter('header', this)">🤸 Headers</button>
</div>

<!-- STATS BAR -->
<div class="stats-bar">
  <div class="stat-cell"><div class="stat-n">24</div><div class="stat-l">Total Clips</div></div>
  <div class="stat-cell"><div class="stat-n">4K</div><div class="stat-l">Max Quality</div></div>
  <div class="stat-cell"><div class="stat-n">8</div><div class="stat-l">Categories</div></div>
  <div class="stat-cell"><div class="stat-n" id="dlCount">1.2K</div><div class="stat-l">Downloads</div></div>
</div>

<!-- FEATURED -->
<div class="featured-wrap">
  <div class="section-eyebrow">Clip of the Day</div>
  <div class="featured-card">
    <div class="featured-video" onclick="openLightbox('cl-01')">
      <span class="featured-emoji">⚽</span>
      <span class="video-corner-badge badge-goal">Goal</span>
      <span class="video-corner-time">0:31</span>
      <div class="big-play">▶</div>
    </div>
    <div class="featured-info">
      <div>
        <div style="font-size:11px; color:var(--dim); font-family:'JetBrains Mono',monospace; text-transform:uppercase; letter-spacing:0.1em; margin-bottom:8px;">Featured</div>
        <div class="featured-title">Bellingham Long Range Screamer vs Bayern</div>
      </div>
      <div class="meta-row">
        <div class="meta-item"><span class="meta-icon">🏟</span> Allianz Arena, Munich</div>
        <div class="meta-item"><span class="meta-icon">📅</span> Champions League · QF 2026</div>
        <div class="meta-item"><span class="meta-icon">📐</span> 1920×1080 · MP4 · 42MB</div>
        <div class="meta-item"><span class="meta-icon">⬇️</span> 847 downloads</div>
      </div>
      <div class="clip-tags">
        <span class="clip-tag">#UCL</span>
        <span class="clip-tag">#bellingham</span>
        <span class="clip-tag">#longrange</span>
        <span class="clip-tag">#realmadrid</span>
      </div>
      <div class="download-options">
        <div class="download-opt" onclick="triggerDownload('Bellingham Scream
