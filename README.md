<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Blockchain Application Security — Learning Companion</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1a2235;
    --border: #1e2d45;
    --accent: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --warn: #f59e0b;
    --danger: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --mono: 'Space Mono', monospace;
    --sans: 'Syne', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Animated grid bg */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 960px;
    margin: 0 auto;
    padding: 0 24px;
    position: relative;
    z-index: 1;
  }

  /* Header */
  header {
    padding: 48px 0 32px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 40px;
  }

  .badge {
    display: inline-block;
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--accent);
    border: 1px solid var(--accent);
    padding: 3px 10px;
    border-radius: 2px;
    margin-bottom: 16px;
    opacity: 0.8;
  }

  h1 {
    font-size: clamp(28px, 5vw, 44px);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.02em;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .subtitle {
    color: var(--muted);
    font-size: 14px;
    font-family: var(--mono);
    margin-top: 10px;
  }

  /* Mode tabs */
  .mode-tabs {
    display: flex;
    gap: 8px;
    margin-bottom: 32px;
    flex-wrap: wrap;
  }

  .tab-btn {
    font-family: var(--mono);
    font-size: 12px;
    letter-spacing: 0.05em;
    padding: 8px 18px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--muted);
    cursor: pointer;
    border-radius: 4px;
    transition: all 0.2s;
  }

  .tab-btn:hover { border-color: var(--accent); color: var(--accent); }
  .tab-btn.active {
    background: var(--accent);
    border-color: var(--accent);
    color: var(--bg);
    font-weight: 700;
  }

  /* Panels */
  .panel { display: none; animation: fadeIn 0.3s ease; }
  .panel.active { display: block; }

  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

  /* Overview cards */
  .overview-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 16px;
    margin-bottom: 32px;
  }

  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 20px;
  }

  .stat-card .label {
    font-family: var(--mono);
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--muted);
    margin-bottom: 6px;
  }

  .stat-card .value {
    font-size: 28px;
    font-weight: 800;
    color: var(--accent);
  }

  .stat-card .desc {
    font-size: 12px;
    color: var(--muted);
    margin-top: 4px;
  }

  /* Chapter cards */
  .chapter-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 24px;
    margin-bottom: 16px;
    cursor: pointer;
    transition: border-color 0.2s, transform 0.2s;
  }

  .chapter-card:hover { border-color: var(--accent2); transform: translateX(4px); }
  .chapter-card.expanded { border-color: var(--accent2); }

  .chapter-header {
    display: flex;
    align-items: flex-start;
    gap: 16px;
  }

  .ch-num {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent2);
    background: rgba(124,58,237,0.1);
    border: 1px solid rgba(124,58,237,0.3);
    padding: 4px 10px;
    border-radius: 4px;
    white-space: nowrap;
    margin-top: 2px;
  }

  .ch-title {
    font-size: 18px;
    font-weight: 700;
    color: var(--text);
    flex: 1;
  }

  .ch-pages {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--muted);
    margin-top: 4px;
  }

  .ch-arrow {
    color: var(--muted);
    font-size: 18px;
    transition: transform 0.2s;
    margin-top: 2px;
  }

  .chapter-card.expanded .ch-arrow { transform: rotate(90deg); }

  .chapter-body {
    display: none;
    margin-top: 16px;
    padding-top: 16px;
    border-top: 1px solid var(--border);
  }

  .chapter-card.expanded .chapter-body { display: block; }

  .section-list {
    list-style: none;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 8px;
    margin-bottom: 16px;
  }

  .section-list li {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    padding: 6px 10px;
    background: var(--surface2);
    border-radius: 4px;
    border-left: 2px solid var(--accent3);
    transition: color 0.2s;
  }

  .section-list li:hover { color: var(--text); }

  .ch-summary {
    font-size: 14px;
    line-height: 1.7;
    color: #94a3b8;
    margin-top: 12px;
  }

  .key-concepts {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-top: 12px;
  }

  .tag {
    font-family: var(--mono);
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 3px;
    border: 1px solid;
  }

  .tag-blue { color: var(--accent); border-color: rgba(0,212,255,0.3); background: rgba(0,212,255,0.05); }
  .tag-purple { color: #a78bfa; border-color: rgba(167,139,250,0.3); background: rgba(167,139,250,0.05); }
  .tag-green { color: var(--accent3); border-color: rgba(16,185,129,0.3); background: rgba(16,185,129,0.05); }
  .tag-orange { color: var(--warn); border-color: rgba(245,158,11,0.3); background: rgba(245,158,11,0.05); }
  .tag-red { color: var(--danger); border-color: rgba(239,68,68,0.3); background: rgba(239,68,68,0.05); }

  /* Glossary */
  .search-bar {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 12px 16px;
    color: var(--text);
    font-family: var(--mono);
    font-size: 13px;
    outline: none;
    margin-bottom: 20px;
    transition: border-color 0.2s;
  }

  .search-bar:focus { border-color: var(--accent); }
  .search-bar::placeholder { color: var(--muted); }

  .glossary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 12px;
  }

  .glossary-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 16px;
    transition: border-color 0.2s;
  }

  .glossary-item:hover { border-color: var(--accent3); }
  .glossary-item.hidden { display: none; }

  .g-term {
    font-family: var(--mono);
    font-size: 12px;
    font-weight: 700;
    color: var(--accent);
    margin-bottom: 6px;
  }

  .g-def {
    font-size: 13px;
    color: #94a3b8;
    line-height: 1.6;
  }

  /* Quiz */
  .quiz-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 28px;
    margin-bottom: 16px;
  }

  .q-label {
    font-family: var(--mono);
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--muted);
    margin-bottom: 12px;
  }

  .q-text {
    font-size: 16px;
    font-weight: 600;
    line-height: 1.5;
    margin-bottom: 20px;
    color: var(--text);
  }

  .q-options {
    display: grid;
    gap: 10px;
  }

  .q-option {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 12px 16px;
    cursor: pointer;
    font-size: 14px;
    color: #94a3b8;
    transition: all 0.2s;
    text-align: left;
  }

  .q-option:hover:not(:disabled) { border-color: var(--accent2); color: var(--text); }
  .q-option.correct { border-color: var(--accent3); color: var(--accent3); background: rgba(16,185,129,0.08); }
  .q-option.wrong { border-color: var(--danger); color: var(--danger); background: rgba(239,68,68,0.08); }
  .q-option:disabled { cursor: default; }

  .q-explanation {
    display: none;
    margin-top: 16px;
    padding: 14px;
    background: rgba(0,212,255,0.05);
    border-left: 3px solid var(--accent);
    border-radius: 0 6px 6px 0;
    font-size: 13px;
    color: #94a3b8;
    line-height: 1.6;
  }

  .q-explanation.show { display: block; animation: fadeIn 0.3s; }

  .score-bar {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 24px;
    display: flex;
    gap: 24px;
    align-items: center;
    flex-wrap: wrap;
  }

  .score-item { text-align: center; }
  .score-num { font-size: 28px; font-weight: 800; color: var(--accent); font-family: var(--mono); }
  .score-lbl { font-size: 11px; color: var(--muted); font-family: var(--mono); text-transform: uppercase; letter-spacing: 0.05em; }

  .btn {
    font-family: var(--mono);
    font-size: 12px;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    border: 1px solid var(--accent);
    background: transparent;
    color: var(--accent);
    transition: all 0.2s;
    letter-spacing: 0.05em;
  }

  .btn:hover { background: var(--accent); color: var(--bg); }
  .btn-primary { background: var(--accent); color: var(--bg); }
  .btn-primary:hover { background: transparent; color: var(--accent); }

  /* Roadmap */
  .roadmap {
    position: relative;
    padding-left: 32px;
  }

  .roadmap::before {
    content: '';
    position: absolute;
    left: 10px;
    top: 8px;
    bottom: 8px;
    width: 2px;
    background: linear-gradient(to bottom, var(--accent), var(--accent2));
  }

  .roadmap-item {
    position: relative;
    margin-bottom: 24px;
  }

  .roadmap-dot {
    position: absolute;
    left: -28px;
    top: 6px;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background: var(--bg);
    border: 2px solid var(--accent);
    transition: background 0.2s;
  }

  .roadmap-item.done .roadmap-dot { background: var(--accent3); border-color: var(--accent3); }
  .roadmap-item.active-item .roadmap-dot { background: var(--accent); box-shadow: 0 0 10px var(--accent); }

  .roadmap-week {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 4px;
  }

  .roadmap-title { font-size: 15px; font-weight: 700; margin-bottom: 6px; }
  .roadmap-topics { font-size: 13px; color: var(--muted); line-height: 1.6; }

  .progress-ring {
    display: inline-block;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background: conic-gradient(var(--accent3) 0%, var(--surface2) 0%);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: var(--mono);
    font-size: 10px;
    color: var(--text);
    font-weight: 700;
    border: 2px solid var(--border);
  }

  footer {
    margin-top: 60px;
    padding: 24px 0;
    border-top: 1px solid var(--border);
    text-align: center;
    font-family: var(--mono);
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 0.05em;
  }
</style>
</head>
<body>

<div class="container">

  <header>
    <div class="badge">Learning Companion</div>
    <h1>Blockchain Application Security</h1>
    <div class="subtitle">Morana · Singh · Piccoli — Wiley, 2026 · 641 pages · 4 chapters + 9 appendices</div>
  </header>

  <div class="mode-tabs">
    <button class="tab-btn active" onclick="switchTab('overview')">📚 Overview</button>
    <button class="tab-btn" onclick="switchTab('chapters')">🗂 Chapters</button>
    <button class="tab-btn" onclick="switchTab('glossary')">🔑 Glossary</button>
    <button class="tab-btn" onclick="switchTab('quiz')">⚡ Quiz</button>
    <button class="tab-btn" onclick="switchTab('roadmap')">🗺 Study Roadmap</button>
  </div>

  <!-- OVERVIEW -->
  <div id="panel-overview" class="panel active">
    <div class="overview-grid">
      <div class="stat-card">
        <div class="label">Total Pages</div>
        <div class="value">641</div>
        <div class="desc">4 core chapters + 9 appendices</div>
      </div>
      <div class="stat-card">
        <div class="label">Primary Focus</div>
        <div class="value" style="font-size:18px;color:var(--accent2)">dApp Security</div>
        <div class="desc">Design, threat model, audit, deploy</div>
      </div>
      <div class="stat-card">
        <div class="label">Key Framework</div>
        <div class="value" style="font-size:18px;color:var(--accent3)">PASTA</div>
        <div class="desc">Process for Attack Simulation & Threat Analysis</div>
      </div>
      <div class="stat-card">
        <div class="label">Authors</div>
        <div class="value" style="font-size:16px;color:var(--warn)">3 Experts</div>
        <div class="desc">Marco Morana · Harpreet Singh · Francesco Piccoli</div>
      </div>
    </div>

    <div class="chapter-card" style="cursor:default;border-color:var(--accent)">
      <div style="font-size:15px;font-weight:700;margin-bottom:12px;color:var(--accent)">📖 What This Book Is About</div>
      <div style="font-size:14px;line-height:1.8;color:#94a3b8">
        This book is a practical, risk-centric guide to securing blockchain applications — specifically <strong style="color:var(--text)">decentralized applications (dApps)</strong>. It bridges the gap between blockchain fundamentals and real-world security engineering, covering everything from cryptographic primitives and consensus algorithms to smart contract vulnerabilities, DeFi exploit case studies, and full threat-modeling walkthroughs. The authors treat security not as an afterthought but as an architectural requirement baked into every design decision.
      </div>
    </div>

    <div class="chapter-card" style="cursor:default;">
      <div style="font-size:15px;font-weight:700;margin-bottom:12px;">🎯 Who Should Read This</div>
      <div class="key-concepts">
        <span class="tag tag-blue">Blockchain Developers</span>
        <span class="tag tag-purple">Security Architects</span>
        <span class="tag tag-green">Smart Contract Auditors</span>
        <span class="tag tag-orange">DevSecOps Engineers</span>
        <span class="tag tag-red">Penetration Testers</span>
        <span class="tag tag-blue">dApp Designers</span>
        <span class="tag tag-purple">Web3 Builders</span>
        <span class="tag tag-green">CTOs / Field CISOs</span>
      </div>
    </div>

    <div class="chapter-card" style="cursor:default;">
      <div style="font-size:15px;font-weight:700;margin-bottom:12px;">⚡ Key Takeaways</div>
      <ul style="list-style:none;display:grid;gap:10px;">
        <li style="font-size:13px;color:#94a3b8;padding:10px 14px;background:var(--surface2);border-radius:6px;border-left:3px solid var(--accent)">
          <strong style="color:var(--text)">Decentralization doesn't eliminate risk</strong> — it spreads it across smart contracts, wallets, oracles, bridges, and consensus mechanisms
        </li>
        <li style="font-size:13px;color:#94a3b8;padding:10px 14px;background:var(--surface2);border-radius:6px;border-left:3px solid var(--accent2)">
          <strong style="color:var(--text)">Security-by-design</strong> is not optional — architecture decisions made early determine exploitability forever (immutable code on-chain!)
        </li>
        <li style="font-size:13px;color:#94a3b8;padding:10px 14px;background:var(--surface2);border-radius:6px;border-left:3px solid var(--accent3)">
          <strong style="color:var(--text)">PASTA threat modeling</strong> provides a 7-stage structured process to identify, analyze, and mitigate blockchain-specific threats
        </li>
        <li style="font-size:13px;color:#94a3b8;padding:10px 14px;background:var(--surface2);border-radius:6px;border-left:3px solid var(--warn)">
          <strong style="color:var(--text)">Real attack case studies</strong> — the 2016 DAO hack, DeFi exploits, wallet drains — show exactly how theory translates to loss of funds
        </li>
      </ul>
    </div>
  </div>

  <!-- CHAPTERS -->
  <div id="panel-chapters" class="panel">

    <div class="chapter-card" onclick="toggleChapter(this)">
      <div class="chapter-header">
        <div class="ch-num">Ch. 1</div>
        <div style="flex:1">
          <div class="ch-title">The Blockchain Technology Primer</div>
          <div class="ch-pages">Pages 1–119 · 14 sections</div>
        </div>
        <div class="ch-arrow">›</div>
      </div>
      <div class="chapter-body">
        <div class="ch-summary">
          Your foundation. Covers DLT vs blockchain, node types (full, light, validator, archive), scalability layers (L1/L2, sharding, channels), hash functions (SHA-256, Keccak-256), digital signatures (ECDSA, EdDSA), Merkle trees, consensus algorithms (PoW, PoS, DPoS, PBFT, Raft), and privacy techniques (zero-knowledge proofs, ring signatures). Also covers cryptocurrencies, digital wallets (hot/cold/hardware), smart contracts, token transactions, DIDs, and the legal/regulatory landscape.
        </div>
        <ul class="section-list" style="margin-top:14px;">
          <li>1.2 History of Blockchain</li>
          <li>1.3 DLT and Blockchain</li>
          <li>1.4 Blockchain Networks</li>
          <li>1.5 Data Structure</li>
          <li>1.5.1 Hash Functions</li>
          <li>1.5.2 Digital Signatures</li>
          <li>1.5.3 Block Structure</li>
          <li>1.5.4 Merkle Trees</li>
          <li>1.5.6 Inherent Security Risks</li>
          <li>1.6 Consensus Algorithms</li>
          <li>1.7 Cryptocurrencies</li>
          <li>1.8 Digital Wallets</li>
          <li>1.9 Digital Transactions</li>
          <li>1.10 Privacy Controls</li>
          <li>1.11 Identity Controls & DIDs</li>
          <li>1.12 Legal & Regulatory</li>
        </ul>
        <div class="key-concepts">
          <span class="tag tag-blue">SHA-256</span>
          <span class="tag tag-blue">ECDSA</span>
          <span class="tag tag-blue">Merkle Trees</span>
          <span class="tag tag-purple">PoW / PoS</span>
          <span class="tag tag-purple">PBFT</span>
          <span class="tag tag-green">Zero-Knowledge Proofs</span>
          <span class="tag tag-green">DIDs</span>
          <span class="tag tag-orange">51% Attack</span>
          <span class="tag tag-red">Sybil Attack</span>
        </div>
      </div>
    </div>

    <div class="chapter-card" onclick="toggleChapter(this)">
      <div class="chapter-header">
        <div class="ch-num">Ch. 2</div>
        <div style="flex:1">
          <div class="ch-title">Designing Secure Decentralized Applications</div>
          <div class="ch-pages">Pages 121–267 · 10 major sections</div>
        </div>
        <div class="ch-arrow">›</div>
      </div>
      <div class="chapter-body">
        <div class="ch-summary">
          The design-phase security chapter. Covers dApp architectures (frontend, smart contracts, oracles, wallets), security requirements elicitation, secure design principles, securing APIs (OWASP API Top 10 for blockchain), protecting secrets and private keys, consensus algorithm vulnerabilities, token standards (ERC-20, ERC-721, ERC-1155), DEX integration security, DID security, and comprehensive smart contract security (reentrancy, overflow, access control, flash loans). Every subsection pairs vulnerability identification with mitigation best practices.
        </div>
        <ul class="section-list" style="margin-top:14px;">
          <li>2.2 dApp Architectures</li>
          <li>2.3 Security Requirements</li>
          <li>2.4.1 Secure Design Principles</li>
          <li>2.4.2 Securing by Design</li>
          <li>2.4.3 Blockchain APIs</li>
          <li>2.4.4 Confidential Data</li>
          <li>2.4.5 Consensus Security</li>
          <li>2.4.6 Key Management</li>
          <li>2.4.7 Token Transactions</li>
          <li>2.4.8 DEX Security</li>
          <li>2.4.9 Digital Identities</li>
          <li>2.4.10 Smart Contracts</li>
        </ul>
        <div class="key-concepts">
          <span class="tag tag-red">Reentrancy</span>
          <span class="tag tag-red">Flash Loan Attacks</span>
          <span class="tag tag-orange">API Vulnerabilities</span>
          <span class="tag tag-orange">Front-Running</span>
          <span class="tag tag-blue">ERC-20 / ERC-721</span>
          <span class="tag tag-purple">Key Management</span>
          <span class="tag tag-green">Security-by-Design</span>
          <span class="tag tag-green">Secure Coding</span>
        </div>
      </div>
    </div>

    <div class="chapter-card" onclick="toggleChapter(this)">
      <div class="chapter-header">
        <div class="ch-num">Ch. 3</div>
        <div style="flex:1">
          <div class="ch-title">Mitigating Blockchain Vulnerabilities</div>
          <div class="ch-pages">Pages 269–459 · Largest chapter</div>
        </div>
        <div class="ch-arrow">›</div>
      </div>
      <div class="chapter-body">
        <div class="ch-summary">
          The deep-dive threat modeling chapter — the intellectual core of the book. Introduces PASTA (Process for Attack Simulation and Threat Analysis) and applies all 7 stages to a real DeFi lending/borrowing dApp case study. Includes full threat matrices, attack trees, vulnerability scoring, risk registers, and mitigation plans. Also covers real security incidents (smart contract exploits, wallet breaches), security-driven tools (static analysis, fuzzing, formal verification), and compliance auditing frameworks.
        </div>
        <ul class="section-list" style="margin-top:14px;">
          <li>3.1.3 Security Incidents & Lessons</li>
          <li>3.2.1 Intro to Threat Modeling</li>
          <li>3.2.2 PASTA Framework (7 Stages)</li>
          <li>Stage I: Business Objectives</li>
          <li>Stage II: Technical Scope</li>
          <li>Stage III: Decomposition</li>
          <li>Stage IV: Threat Analysis</li>
          <li>Stage V: Vulnerability Analysis</li>
          <li>Stage VI: Attack Modeling</li>
          <li>Stage VII: Risk Management</li>
          <li>3.2.4 Security-Driven Tools</li>
          <li>3.3 Compliance Auditing</li>
        </ul>
        <div class="key-concepts">
          <span class="tag tag-purple">PASTA Methodology</span>
          <span class="tag tag-red">DeFi Exploit Analysis</span>
          <span class="tag tag-orange">STRIDE / DREAD</span>
          <span class="tag tag-blue">Attack Trees</span>
          <span class="tag tag-green">Slither / Mythril</span>
          <span class="tag tag-green">Formal Verification</span>
          <span class="tag tag-orange">Risk Registers</span>
          <span class="tag tag-blue">Compliance Frameworks</span>
        </div>
      </div>
    </div>

    <div class="chapter-card" onclick="toggleChapter(this)">
      <div class="chapter-header">
        <div class="ch-num">Ch. 4</div>
        <div style="flex:1">
          <div class="ch-title">Securing Blockchain Applications: Practical Examples</div>
          <div class="ch-pages">Pages 461–495 · Hands-on capstone</div>
        </div>
        <div class="ch-arrow">›</div>
      </div>
      <div class="chapter-body">
        <div class="ch-summary">
          Hands-on implementation. Walks through building a complete ERC-20 dApp on AWS (Token.sol → Lambda → API Gateway → React frontend), then performs a security review of each layer. Followed by comprehensive auditing tutorials: smart contract vulnerabilities (reentrancy, overflow, DoS, access control), automated tools (Slither, Mythril, Echidna), node software auditing, wallet security auditing, and full dApp audit methodology. Ends with reporting frameworks and responsible disclosure best practices.
        </div>
        <ul class="section-list" style="margin-top:14px;">
          <li>4.2 dApp Creation (ERC-20 + AWS)</li>
          <li>4.2.3 API Gateway Setup</li>
          <li>4.2.4 React Frontend</li>
          <li>4.2.5 Security Review</li>
          <li>4.3.3 Smart Contract Auditing</li>
          <li>4.3.3.1 Reentrancy</li>
          <li>4.3.3.2 Integer Over/Underflow</li>
          <li>4.3.3.3 DoS Attacks</li>
          <li>4.3.3.4 Access Control Failures</li>
          <li>4.3.4 Audit Tools & Processes</li>
          <li>4.3.6 Node Software Auditing</li>
          <li>4.3.7 Wallet Auditing</li>
          <li>4.3.8 dApp Auditing</li>
          <li>4.3.9 Reporting Framework</li>
        </ul>
        <div class="key-concepts">
          <span class="tag tag-blue">Solidity / ERC-20</span>
          <span class="tag tag-blue">AWS Lambda</span>
          <span class="tag tag-green">Slither</span>
          <span class="tag tag-green">Mythril</span>
          <span class="tag tag-green">Echidna (Fuzzing)</span>
          <span class="tag tag-orange">Formal Verification</span>
          <span class="tag tag-purple">Audit Reporting</span>
          <span class="tag tag-red">Reentrancy Guard</span>
        </div>
      </div>
    </div>

  </div>

  <!-- GLOSSARY -->
  <div id="panel-glossary" class="panel">
    <input type="text" class="search-bar" placeholder="Search terms... (e.g. reentrancy, PASTA, oracle)" oninput="filterGlossary(this.value)">
    <div class="glossary-grid" id="glossary-grid">
    </div>
  </div>

  <!-- QUIZ -->
  <div id="panel-quiz" class="panel">
    <div class="score-bar">
      <div class="score-item">
        <div class="score-num" id="score-correct">0</div>
        <div class="score-lbl">Correct</div>
      </div>
      <div class="score-item">
        <div class="score-num" id="score-total">0</div>
        <div class="score-lbl">Answered</div>
      </div>
      <div class="score-item">
        <div class="score-num" id="score-pct">—</div>
        <div class="score-lbl">Score</div>
      </div>
      <div style="flex:1"></div>
      <button class="btn" onclick="resetQuiz()">↺ Reset</button>
    </div>
    <div id="quiz-container"></div>
  </div>

  <!-- ROADMAP -->
  <div id="panel-roadmap" class="panel">
    <div class="chapter-card" style="cursor:default;margin-bottom:28px;">
      <div style="font-size:14px;color:#94a3b8;line-height:1.7;">
        A suggested <strong style="color:var(--text)">4-week self-study plan</strong> for working through this 641-page book. Adjust pace based on your existing blockchain and security background. Each week builds on the previous.
      </div>
    </div>

    <div class="roadmap">
      <div class="roadmap-item active-item">
        <div class="roadmap-dot"></div>
        <div class="roadmap-week">Week 1 — Chapter 1</div>
        <div class="roadmap-title">Blockchain Foundations & Cryptography</div>
        <div class="roadmap-topics">
          Read §1.1–1.6: DLT basics, node types, hash functions (SHA-256, Keccak), ECDSA signatures, Merkle trees, block structure. Then §1.6 consensus algorithms — understand PoW, PoS, and BFT variants. <br><br>
          <strong style="color:var(--accent)">Practice:</strong> Draw a Merkle tree by hand. Explain how PoS differs from PoW in terms of security assumptions.<br>
          <strong style="color:var(--accent2)">Key risk to internalize:</strong> §1.5.6 — Inherent Security Risks of Blockchain Technology.
        </div>
      </div>

      <div class="roadmap-item">
        <div class="roadmap-dot"></div>
        <div class="roadmap-week">Week 2 — Chapter 1 (cont.) + Chapter 2 start</div>
        <div class="roadmap-title">Wallets, Transactions, Privacy & dApp Architectures</div>
        <div class="roadmap-topics">
          Complete Ch. 1: §1.7–1.12 covering cryptocurrencies, wallet types, smart contracts, token transactions, ZK proofs, DIDs, and regulatory landscape. Start Ch. 2: §2.1–2.3 on dApp architectures and security requirements elicitation.<br><br>
          <strong style="color:var(--accent)">Practice:</strong> List security requirements for a hypothetical NFT marketplace. Categorize them (confidentiality, integrity, availability, non-repudiation).<br>
          <strong style="color:var(--accent2)">Focus concept:</strong> How DIDs differ from centralized identity systems.
        </div>
      </div>

      <div class="roadmap-item">
        <div class="roadmap-dot"></div>
        <div class="roadmap-week">Week 3 — Chapter 2 (finish) + Chapter 3</div>
        <div class="roadmap-title">Secure Design Patterns + PASTA Threat Modeling</div>
        <div class="roadmap-topics">
          Finish Ch. 2: §2.4 in full — secure design, API security, key management, token standards, DEX security, smart contract vulnerabilities (reentrancy, flash loans, access control). <br>
          Then dive into Ch. 3: §3.1 incident case studies. Work through the PASTA 7-stage model carefully (§3.2.2) and then trace the DeFi case study (§3.2.3) stage by stage.<br><br>
          <strong style="color:var(--accent)">Practice:</strong> Apply PASTA Stage I–III to a simple voting dApp. What are the business objectives? What data flows are in scope?<br>
          <strong style="color:var(--accent2)">Key deliverables in appendices:</strong> Threat Matrix (A), Attack Paths (C), Risk Register (G).
        </div>
      </div>

      <div class="roadmap-item">
        <div class="roadmap-dot"></div>
        <div class="roadmap-week">Week 4 — Chapter 4 + Appendices</div>
        <div class="roadmap-title">Practical Auditing + Hands-On Implementation</div>
        <div class="roadmap-topics">
          Work through Ch. 4 hands-on: build the ERC-20 + AWS example mentally (or in code). Walk through the security review per layer. Then read §4.3 auditing methodology fully — smart contracts, nodes, wallets, dApps.<br>
          Review key appendices: A (Threat Matrix), B (Weaknesses), D (Attack Simulation Tests), H (Attack Simulation Report), I (Risk Analysis Report).<br><br>
          <strong style="color:var(--accent)">Capstone challenge:</strong> Pick an open-source Solidity contract on GitHub. Use Slither on it. Identify one real vulnerability using patterns from the book.<br>
          <strong style="color:var(--accent2)">Tools to try:</strong> Slither (static analysis), Mythril (symbolic execution), Echidna (fuzzing).
        </div>
      </div>
    </div>

    <div class="chapter-card" style="cursor:default;margin-top:24px;">
      <div style="font-size:14px;font-weight:700;margin-bottom:10px;color:var(--accent3)">🛠 Recommended Tools to Install</div>
      <div class="key-concepts">
        <span class="tag tag-green">Slither (Trail of Bits)</span>
        <span class="tag tag-green">Mythril</span>
        <span class="tag tag-green">Echidna</span>
        <span class="tag tag-blue">Hardhat</span>
        <span class="tag tag-blue">Foundry</span>
        <span class="tag tag-blue">Remix IDE</span>
        <span class="tag tag-purple">OpenZeppelin Contracts</span>
        <span class="tag tag-orange">Ganache (local chain)</span>
      </div>
    </div>
  </div>

  <footer>Blockchain Application Security · Learning Companion · Built with Claude</footer>
</div>

<script>
function switchTab(id) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('panel-' + id).classList.add('active');
  event.target.classList.add('active');
  if (id === 'glossary') renderGlossary();
  if (id === 'quiz') renderQuiz();
}

function toggleChapter(el) {
  el.classList.toggle('expanded');
}

// GLOSSARY DATA
const glossaryData = [
  { term: "dApp", def: "Decentralized Application — runs on a blockchain network, with logic in smart contracts and no central point of control." },
  { term: "Smart Contract", def: "Self-executing code stored on-chain. Once deployed, it is immutable. Bugs cannot be patched without redeployment." },
  { term: "Reentrancy", def: "A smart contract vulnerability where an external call re-enters the calling contract before its state is updated, enabling fund drain attacks (e.g., the 2016 DAO hack)." },
  { term: "PASTA", def: "Process for Attack Simulation and Threat Analysis — a 7-stage risk-centric threat modeling framework adapted for blockchain applications in Chapter 3." },
  { term: "Merkle Tree", def: "A binary hash tree where each leaf is a transaction hash and each parent is the hash of its children. Used to efficiently verify transaction inclusion without downloading the full block." },
  { term: "Consensus Algorithm", def: "The protocol by which distributed nodes agree on the canonical state of the ledger. Examples: PoW, PoS, PBFT, Raft, DPoS." },
  { term: "51% Attack", def: "When a single entity controls >50% of mining/staking power and can rewrite recent transaction history or double-spend." },
  { term: "Oracle", def: "An off-chain data feed that supplies real-world information (prices, events) to smart contracts. A key attack surface since oracles are not trustless." },
  { term: "Flash Loan Attack", def: "A DeFi exploit that borrows large uncollateralized sums within a single transaction to manipulate prices or drain liquidity pools." },
  { term: "Front-Running", def: "A miner or bot sees a pending transaction and inserts their own transaction first (higher gas fee) to exploit the information advantage." },
  { term: "ERC-20", def: "The Ethereum fungible token standard defining transfer(), approve(), and allowance() methods. Vulnerabilities include approve/transferFrom race conditions." },
  { term: "ERC-721", def: "Ethereum non-fungible token (NFT) standard — each token has a unique ID. Used in digital art, gaming, and ownership certificates." },
  { term: "DID", def: "Decentralized Identifier — a self-sovereign identifier that doesn't depend on any centralized authority, enabling privacy-preserving identity." },
  { term: "Zero-Knowledge Proof", def: "A cryptographic method allowing one party to prove they know a value without revealing the value itself. Used for privacy-preserving transactions (e.g., zk-SNARKs)." },
  { term: "ECDSA", def: "Elliptic Curve Digital Signature Algorithm — used by Bitcoin and Ethereum to sign transactions, proving private key ownership without revealing it." },
  { term: "Sybil Attack", def: "An attacker creates many fake identities (nodes) to gain disproportionate influence in a P2P network, disrupting consensus or reputation systems." },
  { term: "Slither", def: "A Solidity static analysis framework by Trail of Bits that detects vulnerabilities like reentrancy, unchecked return values, and shadowed variables." },
  { term: "Mythril", def: "A symbolic execution tool for EVM bytecode that explores execution paths to find vulnerabilities like integer overflows, reentrancy, and dangerous delegatecall." },
  { term: "Formal Verification", def: "Mathematically proving that a smart contract behaves according to its specification under all possible inputs — the highest assurance level." },
  { term: "PBFT", def: "Practical Byzantine Fault Tolerance — a deterministic consensus algorithm tolerant of up to 1/3 malicious nodes. Used in permissioned blockchains (Hyperledger Fabric)." },
  { term: "Integer Overflow", def: "In Solidity <0.8.0, arithmetic can silently wrap around. E.g., uint8 at 255 + 1 = 0. Mitigated by SafeMath or Solidity 0.8+ built-in checks." },
  { term: "Access Control", def: "Restricting who can call which smart contract functions. Missing access control is a top vulnerability — any user can call admin-only functions." },
  { term: "DEX", def: "Decentralized Exchange — a protocol (Uniswap, Curve) allowing trustless token swaps via liquidity pools and automated market makers (AMMs)." },
  { term: "Hot Wallet", def: "A digital wallet connected to the internet. Convenient for frequent use but higher risk due to exposure to online attacks." },
  { term: "Cold Wallet", def: "Offline storage of private keys (hardware wallet like Ledger, or paper wallet). Safer from remote attacks but vulnerable to physical theft." },
  { term: "Gas Griefing", def: "A DoS pattern where a malicious contract's fallback function consumes all gas, causing the calling contract's transaction to fail." },
];

let glossaryRendered = false;
function renderGlossary() {
  if (glossaryRendered) return;
  const grid = document.getElementById('glossary-grid');
  grid.innerHTML = glossaryData.map((item, i) => `
    <div class="glossary-item" data-term="${item.term.toLowerCase()}">
      <div class="g-term">${item.term}</div>
      <div class="g-def">${item.def}</div>
    </div>
  `).join('');
  glossaryRendered = true;
}

function filterGlossary(q) {
  if (!glossaryRendered) renderGlossary();
  q = q.toLowerCase();
  document.querySelectorAll('.glossary-item').forEach(el => {
    const term = el.getAttribute('data-term');
    const def = el.querySelector('.g-def').textContent.toLowerCase();
    el.classList.toggle('hidden', q.length > 0 && !term.includes(q) && !def.includes(q));
  });
}

// QUIZ DATA
const questions = [
  {
    ch: "Ch. 1",
    q: "In a Merkle tree, what does the root hash represent?",
    options: ["The hash of the most recent transaction", "A cryptographic summary of all transactions in a block", "The private key of the block validator", "The nonce used in PoW mining"],
    answer: 1,
    exp: "The Merkle root is derived by hashing pairs of transaction hashes recursively up the tree. Changing any single transaction changes the root, making tampering detectable. Light clients use Merkle proofs to verify transactions without downloading the full block."
  },
  {
    ch: "Ch. 1",
    q: "Which attack involves an adversary controlling more than half of a blockchain's computational or staking power?",
    options: ["Sybil Attack", "Eclipse Attack", "51% Attack", "Front-Running Attack"],
    answer: 2,
    exp: "A 51% attack lets the majority actor reorganize recent blocks, enabling double-spends and censorship. It's more practical on small PoW chains (low hash rate) than on large ones like Bitcoin. PoS chains have different but analogous vulnerabilities around stake concentration."
  },
  {
    ch: "Ch. 1",
    q: "What distinguishes a DID (Decentralized Identifier) from a traditional identity credential?",
    options: ["DIDs require a government-issued document", "DIDs are controlled by a central authority", "DIDs are self-sovereign — controlled by the user without any central registry", "DIDs are only usable on the Ethereum blockchain"],
    answer: 2,
    exp: "DIDs are anchored on a blockchain or DLT, meaning no central party can revoke or alter them without the holder's consent. They're key to self-sovereign identity (SSI) systems and privacy-preserving authentication without username/password."
  },
  {
    ch: "Ch. 2",
    q: "The 2016 DAO hack exploited which smart contract vulnerability?",
    options: ["Integer overflow in token balance", "Reentrancy — withdraw() was called recursively before state was updated", "A missing require() check on access control", "Front-running via mempool observation"],
    answer: 1,
    exp: "The attacker called the withdraw function, which sent ETH before updating the balance. The attacker's fallback function called withdraw again recursively, draining ~$50M. The fix: always update state BEFORE making external calls (Checks-Effects-Interactions pattern)."
  },
  {
    ch: "Ch. 2",
    q: "In Solidity versions before 0.8.0, what happens when a uint8 variable at value 255 has 1 added to it?",
    options: ["An exception is thrown", "The value becomes 256", "The value wraps around to 0 (integer overflow)", "The transaction reverts automatically"],
    answer: 2,
    exp: "Pre-0.8.0 Solidity had no built-in overflow protection. Uint8 can only hold 0–255; adding 1 wraps to 0. This was exploited in early token contracts (e.g., BEC token). Mitigations: use SafeMath library or upgrade to Solidity ≥0.8.0 where overflow reverts automatically."
  },
  {
    ch: "Ch. 2",
    q: "What is an oracle in the context of blockchain/dApps?",
    options: ["A type of consensus algorithm for permissioned chains", "An off-chain data provider that feeds real-world information to smart contracts", "A hardware security module for storing private keys", "A specialized node that validates cross-chain bridges"],
    answer: 1,
    exp: "Oracles bridge the on-chain and off-chain worlds. Price oracles (Chainlink, Band Protocol) are a top DeFi attack surface — manipulating the price feed (e.g., via flash loans) can exploit any contract that uses that price for liquidations or collateral valuation."
  },
  {
    ch: "Ch. 3",
    q: "In the PASTA threat modeling framework, how many stages does the methodology have?",
    options: ["3 stages", "5 stages", "7 stages", "9 stages"],
    answer: 2,
    exp: "PASTA's 7 stages are: (I) Business Objectives, (II) Technical Scope, (III) Application Decomposition, (IV) Threat Analysis, (V) Vulnerability Analysis, (VI) Attack Modeling, and (VII) Risk Assessment & Mitigation. Chapter 3 applies all 7 to a DeFi lending dApp in detail."
  },
  {
    ch: "Ch. 3",
    q: "What is front-running in a blockchain context?",
    options: ["Submitting a transaction after seeing another user's pending transaction in the mempool and paying higher gas to be included first", "Deploying a smart contract before another team can", "Mining blocks before full validation", "Bypassing signature verification in token transfers"],
    answer: 0,
    exp: "Since pending transactions are visible in the public mempool, bots and miners can see your transaction and insert their own with higher gas fees to execute first. In DeFi this is used to exploit DEX trades (sandwich attacks). MEV (Miner Extractable Value) is the formal term for this economic phenomenon."
  },
  {
    ch: "Ch. 4",
    q: "Which static analysis tool by Trail of Bits is specifically designed to detect Solidity vulnerabilities?",
    options: ["Echidna", "Mythril", "Slither", "Manticore"],
    answer: 2,
    exp: "Slither is Trail of Bits' Python-based Solidity analyzer that runs quickly (seconds) and detects common issues like reentrancy, unchecked sends, and tainted variables. Echidna is their fuzzer. Mythril uses symbolic execution. Chapter 4 covers these tools in the audit process context."
  },
  {
    ch: "Ch. 4",
    q: "The Checks-Effects-Interactions (CEI) pattern in Solidity is the primary defense against which vulnerability?",
    options: ["Integer overflow", "Reentrancy attacks", "Access control failures", "Gas limit DoS"],
    answer: 1,
    exp: "CEI mandates: (1) Checks — validate all conditions first, (2) Effects — update state variables, (3) Interactions — make external calls last. This prevents reentrancy because when the external call re-enters, the state has already been updated (e.g., balance set to 0 before sending ETH)."
  },
];

let quizState = { answered: 0, correct: 0 };

function renderQuiz() {
  const container = document.getElementById('quiz-container');
  if (container.children.length > 0) return;
  questions.forEach((q, i) => {
    container.innerHTML += `
      <div class="quiz-card" id="qcard-${i}">
        <div class="q-label">${q.ch} · Question ${i+1} of ${questions.length}</div>
        <div class="q-text">${q.q}</div>
        <div class="q-options">
          ${q.options.map((opt, j) => `<button class="q-option" onclick="answer(${i},${j})" id="qopt-${i}-${j}">${opt}</button>`).join('')}
        </div>
        <div class="q-explanation" id="qexp-${i}">${q.exp}</div>
      </div>
    `;
  });
}

function answer(qi, selected) {
  const q = questions[qi];
  const opts = document.querySelectorAll(`#qcard-${qi} .q-option`);
  opts.forEach(o => o.disabled = true);
  opts[selected].classList.add(selected === q.answer ? 'correct' : 'wrong');
  if (selected !== q.answer) opts[q.answer].classList.add('correct');
  document.getElementById(`qexp-${qi}`).classList.add('show');
  quizState.answered++;
  if (selected === q.answer) quizState.correct++;
  document.getElementById('score-correct').textContent = quizState.correct;
  document.getElementById('score-total').textContent = quizState.answered;
  document.getElementById('score-pct').textContent = Math.round(quizState.correct / quizState.answered * 100) + '%';
}

function resetQuiz() {
  quizState = { answered: 0, correct: 0 };
  document.getElementById('quiz-container').innerHTML = '';
  document.getElementById('score-correct').textContent = '0';
  document.getElementById('score-total').textContent = '0';
  document.getElementById('score-pct').textContent = '—';
  renderQuiz();
}
</script>
</body>
</html>
