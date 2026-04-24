---
permalink: /
title: ""
excerpt: "Linhan Xia — Ph.D. Student at the University of Oklahoma"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<style>
  /* ---------- Homepage-scoped styling ---------- */
  .hp-hero {
    margin-top: 0.25em;
    margin-bottom: 1.6em;
  }
  .hp-hero h1 {
    font-size: 2em;
    margin: 0 0 0.15em 0;
    font-weight: 700;
    letter-spacing: -0.5px;
  }
  .hp-hero .hp-sub {
    font-size: 1.05em;
    color: #666;
    margin: 0;
  }
  .hp-hero .hp-accent {
    display: inline-block;
    width: 44px;
    height: 4px;
    border-radius: 2px;
    background: linear-gradient(90deg, #841617 0%, #c96b1e 100%);
    margin: 0.7em 0 1em 0;
  }
  .hp-section-title {
    display: flex;
    align-items: center;
    gap: 0.45em;
    font-size: 1.25em;
    font-weight: 700;
    margin: 1.8em 0 0.75em 0;
    padding-bottom: 0.35em;
    border-bottom: 1px solid #e6e6e6;
    color: #222;
  }
  .hp-section-title .hp-ico { font-size: 1.1em; }
  .hp-bio p { line-height: 1.7; margin: 0 0 0.9em 0; }
  .hp-interests {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 0.8em;
    margin: 0.6em 0 0 0;
  }
  .hp-card {
    background: #fafafa;
    border: 1px solid #ececec;
    border-left: 3px solid #841617;
    border-radius: 6px;
    padding: 0.8em 0.95em;
    transition: transform 0.15s ease, box-shadow 0.15s ease;
  }
  .hp-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 14px rgba(0,0,0,0.06);
  }
  .hp-card-title {
    font-weight: 600;
    font-size: 0.98em;
    margin-bottom: 0.25em;
    color: #222;
  }
  .hp-card-desc {
    font-size: 0.88em;
    color: #666;
    line-height: 1.5;
  }
  .hp-news { margin: 0.5em 0 0 0; padding: 0; list-style: none; }
  .hp-news li {
    position: relative;
    padding: 0.75em 0.95em 0.75em 1em;
    margin-bottom: 0.6em;
    background: #fafafa;
    border-left: 3px solid #c96b1e;
    border-radius: 0 6px 6px 0;
    line-height: 1.55;
    font-size: 0.95em;
  }
  .hp-news .hp-date {
    display: inline-block;
    font-weight: 700;
    color: #841617;
    margin-right: 0.4em;
    font-size: 0.88em;
    letter-spacing: 0.2px;
  }
  .hp-news .hp-badge {
    display: inline-block;
    background: #841617;
    color: #fff;
    font-size: 0.72em;
    font-weight: 700;
    padding: 0.12em 0.55em;
    border-radius: 10px;
    margin-right: 0.35em;
    vertical-align: 2px;
    letter-spacing: 0.4px;
  }
  .hp-edu li {
    padding: 0.45em 0;
    border-bottom: 1px dashed #ececec;
    list-style: none;
  }
  .hp-edu { padding-left: 0; }
  .hp-edu li:last-child { border-bottom: none; }
  .hp-edu .hp-edu-school { font-weight: 600; color: #222; }
  .hp-edu .hp-edu-meta { color: #777; font-size: 0.88em; }
  .hp-cta {
    margin-top: 1.5em;
    padding: 1em 1.2em;
    background: linear-gradient(135deg, #fff7ee 0%, #fdeee6 100%);
    border-radius: 8px;
    font-size: 0.98em;
    color: #333;
  }
  .hp-cta a { font-weight: 600; }

  /* ---------- Publications ---------- */
  .hp-pub-group { margin-bottom: 1.4em; }
  .hp-pub-group-title {
    font-size: 0.8em;
    font-weight: 700;
    color: #555;
    text-transform: uppercase;
    letter-spacing: 1.3px;
    margin: 1.1em 0 0.6em 0;
    padding-left: 0.6em;
    border-left: 3px solid #c96b1e;
  }
  .hp-pub-list {
    list-style: none;
    padding: 0;
    margin: 0;
    counter-reset: pubcounter;
  }
  .hp-pub {
    position: relative;
    counter-increment: pubcounter;
    padding: 0.75em 0.95em 0.8em 2.6em;
    margin-bottom: 0.55em;
    background: #fafafa;
    border: 1px solid #ececec;
    border-radius: 6px;
    line-height: 1.55;
    font-size: 0.93em;
    transition: box-shadow 0.15s ease, border-color 0.15s ease, transform 0.15s ease;
  }
  .hp-pub::before {
    content: counter(pubcounter);
    position: absolute;
    left: 0.7em;
    top: 0.85em;
    width: 1.5em;
    height: 1.5em;
    text-align: center;
    line-height: 1.5em;
    font-size: 0.75em;
    font-weight: 700;
    color: #fff;
    background: #841617;
    border-radius: 50%;
  }
  .hp-pub:hover {
    border-color: #c96b1e;
    box-shadow: 0 3px 12px rgba(0,0,0,0.06);
    transform: translateY(-1px);
  }
  .hp-pub-authors {
    color: #555;
    font-size: 0.88em;
    margin-bottom: 0.25em;
  }
  .hp-pub-authors strong { color: #841617; }
  .hp-pub-title {
    font-weight: 600;
    color: #222;
    margin-bottom: 0.35em;
    line-height: 1.45;
  }
  .hp-pub-venue {
    font-size: 0.85em;
    color: #666;
    font-style: italic;
  }
  .hp-pub-badge {
    display: inline-block;
    font-size: 0.7em;
    font-weight: 700;
    padding: 0.14em 0.6em;
    border-radius: 10px;
    margin-right: 0.3em;
    vertical-align: 1px;
    letter-spacing: 0.5px;
    font-style: normal;
    text-transform: uppercase;
    white-space: nowrap;
  }
  .hp-pub-badge-review  { background: #e3e3e3; color: #555; }
  .hp-pub-badge-ccfb    { background: #841617; color: #fff; }
  .hp-pub-badge-ccfc    { background: #c96b1e; color: #fff; }
  .hp-pub-badge-ei      { background: #4a6b8a; color: #fff; }
  .hp-pub-badge-patent  { background: #b8860b; color: #fff; }
  .hp-pub-badge-arxiv   { background: #6a4c93; color: #fff; }
  .hp-pub-badge-accepted { background: #2e8b57; color: #fff; }
</style>

<div class="hp-hero" markdown="1">

# 👋 Hi, I'm Linhan Xia

<p class="hp-sub">Ph.D. Student · School of Industrial &amp; Systems Engineering · University of Oklahoma</p>

<span class="hp-accent"></span>

</div>

<div class="hp-bio" markdown="1">

I'm **Linhan Xia** (夏霖翰) — you can also call me **Chris**. I was born and raised in the People's Republic of China, and I received my bachelor's degree from **Hong Kong Baptist University**. I am currently pursuing a Ph.D. at the **University of Oklahoma**, where my work centers on model-assisted CAD generation.

Before joining OU, I was a research assistant at **ICNLab, Peking University Shenzhen Graduate School**, working with Prof. [Kai Lei](https://www.researchgate.net/profile/Kai-Lei/2). During my undergraduate years, I worked as a research assistant at **Beijing Normal University–Hong Kong Baptist University United International College (UIC)** under the supervision of Prof. [Ricky Yuen-tan Hou](https://staff.uic.edu.cn/rickyhou/en).

</div>

<div class="hp-section-title"><span class="hp-ico">🔬</span> Research Interests</div>

<div class="hp-interests">
  <div class="hp-card">
    <div class="hp-card-title">Data-driven modeling</div>
    <div class="hp-card-desc">Continuous-time neural architectures, sequence modeling, and representation learning.</div>
  </div>
  <div class="hp-card">
    <div class="hp-card-title">Time-Series Forecasting</div>
    <div class="hp-card-desc">Benchmarking and designing models for high-frequency and limit-order-book data.</div>
  </div>
  <div class="hp-card">
    <div class="hp-card-title">Financial Data Analytics</div>
    <div class="hp-card-desc">Crypto market microstructure, quantitative signals, and systematic trading research.</div>
  </div>
</div>

<div class="hp-section-title"><span class="hp-ico">📰</span> News</div>

<ul class="hp-news">
  <li>
    <span class="hp-date">Apr 2026</span>
    <span class="hp-badge">Accepted</span>
    🎉 Our paper <strong>"Liquid Time Constant Neural Networks for Crypto LOB Prediction: A Benchmark Study"</strong> (Paper&nbsp;#5901) has been accepted by <strong>KSEM 2026</strong> as an <strong>AICom2</strong> paper. Many thanks to the program committee and all collaborators!
  </li>
</ul>

<div class="hp-section-title"><span class="hp-ico">📚</span> Publications</div>

<p style="font-size:0.85em; color:#777; margin: 0 0 0.6em 0;">
  <strong style="color:#841617;">Bold</strong> indicates myself; <code style="background:#f2f2f2; padding:0 0.3em; border-radius:3px;">*</code> indicates corresponding author.
</p>

<div class="hp-pub-group">
  <div class="hp-pub-group-title">Under Review / Preprints</div>
  <ol class="hp-pub-list">
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Rui Zhu, Shivakumar Raman, Lee Graves, Yifu Li*</div>
      <div class="hp-pub-title">VeriCAD: Compiler-Grounded Reinforcement Learning for Executable and Aligned Text-to-CAD Generation</div>
      <div class="hp-pub-venue"><span class="hp-pub-badge hp-pub-badge-review">Under Review</span></div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Rui Zhu, Shivakumar Raman, Lee Graves, Yifu Li*</div>
      <div class="hp-pub-title">CPD-LLM: Customized Product Design (CPD) via a Design Syntax-Aware Large Language Model (LLM)</div>
      <div class="hp-pub-venue"><span class="hp-pub-badge hp-pub-badge-review">Under Review</span></div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors">Qiankang Xv, Haixiao Hu, <strong>Linhan Xia</strong>, Jingjing Wang, Weifan Lin, Yu Guo, Xianneng Zou, Kai Lei*</div>
      <div class="hp-pub-title">Mamba4Net-MoE: Efficient Multi-Task Network Intelligence via Mixture-of-Experts Distillation on Linear State Space Models</div>
      <div class="hp-pub-venue"><span class="hp-pub-badge hp-pub-badge-review">Under Review</span></div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Yicheng Yang, Ziou Chen, Zheng Yang, Shengxin Zhu*</div>
      <div class="hp-pub-title">Movie Recommendation with Poster Attention via Multi-modal Transformer Feature Fusion</div>
      <div class="hp-pub-venue"><span class="hp-pub-badge hp-pub-badge-arxiv">arXiv Preprint</span></div>
    </li>
  </ol>
</div>

<div class="hp-pub-group">
  <div class="hp-pub-group-title">Conference Papers</div>
  <ol class="hp-pub-list">
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Mingzhan Yang, Jingjing Wang, Ziwei Yan, Yakun Ren, Guo Yu, Kai Lei*</div>
      <div class="hp-pub-title">Mamba4Net: Distilled Hybrid Mamba Large Language Models for Networking</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ccfb">CCF-B</span>
        <span class="hp-pub-badge hp-pub-badge-ei">EI</span>
        The 33rd IEEE International Conference on Network Protocols (ICNP 2025)
      </div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Guohui Yuan, Shengnan Tao, Yujing Qiu, Guo Yu, Kai Lei*</div>
      <div class="hp-pub-title">Fine-tuned Poly Encoders for Word Sense Disambiguation</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ccfc">CCF-C</span>
        The 18th International Conference on Knowledge Science, Engineering and Management (KSEM 2025)
      </div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia</strong>, Jiaxin Cai, Ricky Yuen-Tan Hou*, Seon-Phil Jeong</div>
      <div class="hp-pub-title">Quantification and Validation for Degree of Understanding in M2M Semantic Communications</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ei">EI</span>
        24th International Conference on Communication Technology (ICCT)
      </div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia*</strong>, Junbang Liu, Tong Wu</div>
      <div class="hp-pub-title">Depth Estimation Algorithm Based on Transformer-Encoder and Feature Fusion</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ei">EI</span>
        7th International Conference on Advanced Algorithms and Control Engineering (AACE)
      </div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia*</strong>, Jinyuan Zhang, Bohan Wen</div>
      <div class="hp-pub-title">Optimization Decision Model of Vegetable Stock and Pricing Based on TCN-Attention and Genetic Algorithm</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ei">EI</span>
        4th International Conference on Computer Science and Management Technology (ICCSMT)
      </div>
    </li>
    <li class="hp-pub">
      <div class="hp-pub-authors"><strong>Linhan Xia*</strong></div>
      <div class="hp-pub-title">Chinese Financial Comments Sentiment Detection Based on the Bert-TCN Model with HowNet Disambiguation</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-ei">EI</span>
        3rd International Conference on Digital Economy and Computer Application (ICDECA)
      </div>
    </li>
  </ol>
</div>

<div class="hp-pub-group">
  <div class="hp-pub-group-title">Patents</div>
  <ol class="hp-pub-list">
    <li class="hp-pub">
      <div class="hp-pub-authors">Kai Lei, Jie Jiang, <strong>Linhan Xia</strong>, Xianneng Zou, Chenghao Ma, Shikun Zhang, Bing Cui, Ziwei Yan, Haiyang Zheng</div>
      <div class="hp-pub-title">Network Task Model Construction Method Based on Cross-Architecture Knowledge Distillation</div>
      <div class="hp-pub-venue">
        <span class="hp-pub-badge hp-pub-badge-patent">Patent</span>
        Chinese Patent No. CN121396808A
      </div>
    </li>
  </ol>
</div>

<div class="hp-section-title"><span class="hp-ico">🎓</span> Education</div>

<ul class="hp-edu">
  <li>
    <span class="hp-edu-school">University of Oklahoma</span> — Ph.D. in Industrial &amp; Systems Engineering
    <div class="hp-edu-meta">Norman, OK, USA · In progress</div>
  </li>
  <li>
    <span class="hp-edu-school">Hong Kong Baptist University (UIC)</span> — B.Sc.
    <div class="hp-edu-meta">Zhuhai, China</div>
  </li>
</ul>

<div class="hp-cta" markdown="1">

💬 **Let's connect.** If you are interested in my research, exploring potential collaborations, or just want to say hi, feel free to reach out at [linhan.xia@ou.edu](mailto:linhan.xia@ou.edu).

</div>
