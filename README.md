# <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Inner Voice — AI Companion Design</title>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg: #f7f4ef;
      --bg-card: #f0ece4;
      --ink: #2a2520;
      --ink-soft: #6b6258;
      --accent: #5a7a5e;
      --accent-light: #c8d9c9;
      --accent-warm: #b5804a;
      --rule: #d9d3c9;
      --serif: 'Cormorant Garamond', Georgia, serif;
      --sans: 'DM Sans', sans-serif;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--ink);
      font-family: var(--sans);
      font-weight: 300;
      line-height: 1.8;
      font-size: 16px;
    }

    /* ── NOISE OVERLAY ── */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 0;
      opacity: 0.4;
    }

    /* ── HEADER ── */
    header {
      max-width: 720px;
      margin: 0 auto;
      padding: 80px 32px 64px;
      position: relative;
      animation: fadeUp 0.9s ease both;
    }

    .eyebrow {
      font-family: var(--sans);
      font-size: 11px;
      font-weight: 500;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 20px;
    }

    h1 {
      font-family: var(--serif);
      font-size: clamp(42px, 7vw, 72px);
      font-weight: 300;
      line-height: 1.08;
      letter-spacing: -0.01em;
      color: var(--ink);
      margin-bottom: 12px;
    }

    h1 em {
      font-style: italic;
      color: var(--accent);
    }

    .subtitle {
      font-family: var(--serif);
      font-size: 20px;
      font-weight: 300;
      font-style: italic;
      color: var(--ink-soft);
      margin-top: 16px;
      max-width: 500px;
      line-height: 1.6;
    }

    .header-rule {
      width: 48px;
      height: 1px;
      background: var(--accent);
      margin: 32px 0;
    }

    .header-meta {
      font-size: 13px;
      color: var(--ink-soft);
      letter-spacing: 0.04em;
    }

    /* ── MAIN LAYOUT ── */
    main {
      max-width: 720px;
      margin: 0 auto;
      padding: 0 32px 120px;
    }

    /* ── SECTIONS ── */
    section {
      margin-bottom: 72px;
      animation: fadeUp 0.9s ease both;
    }

    section:nth-child(2) { animation-delay: 0.1s; }
    section:nth-child(3) { animation-delay: 0.2s; }
    section:nth-child(4) { animation-delay: 0.3s; }
    section:nth-child(5) { animation-delay: 0.4s; }

    .section-label {
      font-family: var(--sans);
      font-size: 10px;
      font-weight: 500;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 24px;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .section-label::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--rule);
    }

    h2 {
      font-family: var(--serif);
      font-size: clamp(26px, 4vw, 36px);
      font-weight: 400;
      line-height: 1.2;
      margin-bottom: 20px;
      color: var(--ink);
    }

    h3 {
      font-family: var(--serif);
      font-size: 20px;
      font-weight: 500;
      margin-bottom: 12px;
      color: var(--ink);
    }

    p {
      color: var(--ink-soft);
      margin-bottom: 16px;
      font-size: 15.5px;
    }

    p:last-child { margin-bottom: 0; }

    /* ── PRINCIPLE CARDS ── */
    .principles {
      display: grid;
      gap: 2px;
      margin-top: 32px;
    }

    .principle {
      background: var(--bg-card);
      padding: 28px 32px;
      border-left: 2px solid var(--accent-light);
      transition: border-color 0.2s ease;
    }

    .principle:hover {
      border-color: var(--accent);
    }

    .principle-number {
      font-family: var(--serif);
      font-size: 13px;
      color: var(--accent);
      font-style: italic;
      margin-bottom: 8px;
    }

    .principle h3 {
      font-size: 17px;
      margin-bottom: 8px;
    }

    .principle p {
      font-size: 14.5px;
      margin: 0;
    }

    /* ── SYSTEM PROMPT BLOCK ── */
    .prompt-container {
      background: var(--ink);
      border-radius: 4px;
      overflow: hidden;
      margin-top: 32px;
    }

    .prompt-header {
      background: #1e1a16;
      padding: 12px 20px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .prompt-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: var(--rule);
      opacity: 0.4;
    }

    .prompt-label {
      font-size: 11px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: #8a8078;
      margin-left: 8px;
    }

    .prompt-body {
      padding: 32px;
      font-family: 'Courier New', monospace;
      font-size: 13.5px;
      line-height: 1.9;
      color: #c8c0b4;
    }

    .prompt-section-title {
      color: var(--accent-light);
      font-weight: bold;
      display: block;
      margin-top: 24px;
      margin-bottom: 4px;
      font-size: 11px;
      letter-spacing: 0.15em;
      text-transform: uppercase;
    }

    .prompt-section-title:first-child { margin-top: 0; }

    .prompt-body em {
      color: #e8c99a;
      font-style: normal;
    }

    /* ── DIALOGUE COMPARISON ── */
    .dialogue-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-top: 32px;
    }

    @media (max-width: 600px) {
      .dialogue-grid { grid-template-columns: 1fr; }
    }

    .dialogue-col {
      background: var(--bg-card);
      padding: 28px;
      position: relative;
    }

    .dialogue-col-label {
      font-size: 10px;
      font-weight: 500;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .dialogue-col-label.before { color: #b07070; }
    .dialogue-col-label.after { color: var(--accent); }

    .dialogue-col-label::before {
      content: '';
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: currentColor;
    }

    .bubble {
      margin-bottom: 14px;
      font-size: 14px;
      line-height: 1.65;
    }

    .bubble-role {
      font-size: 10px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--ink-soft);
      margin-bottom: 4px;
      font-weight: 500;
    }

    .bubble-text {
      backgrportfolio
