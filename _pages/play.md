---
permalink: /play/
title: ""
excerpt: "Tetris — a small game for a small break"
author_profile: false
layout: single
---

<div class="hp-hero hp-play-hero" markdown="1">

# 🎮 Coffee-Break Tetris

<p class="hp-sub">Bored of reading my publications? Stack some bricks. Use ←/→ to move, ↑ to rotate, ↓ for soft drop, Space for hard drop, P to pause.</p>

<span class="hp-accent"></span>

</div>

<div class="tetris-arena">
  <div class="tetris-grid">
    <div class="tetris-col tetris-col--left">
      <div class="tetris-stat">
        <span class="tetris-stat__label">Score</span>
        <span class="tetris-stat__value" id="t-score">0</span>
      </div>
      <div class="tetris-stat">
        <span class="tetris-stat__label">Lines</span>
        <span class="tetris-stat__value" id="t-lines">0</span>
      </div>
      <div class="tetris-stat">
        <span class="tetris-stat__label">Level</span>
        <span class="tetris-stat__value" id="t-level">1</span>
      </div>
      <div class="tetris-stat tetris-stat--best">
        <span class="tetris-stat__label">Best</span>
        <span class="tetris-stat__value" id="t-best">0</span>
      </div>
    </div>

    <div class="tetris-col tetris-col--center">
      <div class="tetris-board-wrap">
        <canvas id="t-board" width="280" height="560" aria-label="Tetris board"></canvas>
        <div class="tetris-overlay" id="t-overlay">
          <div class="tetris-overlay__title" id="t-overlay-title">Tetris</div>
          <div class="tetris-overlay__msg" id="t-overlay-msg">Press <kbd>Space</kbd> or tap Start</div>
          <button class="tetris-btn" id="t-start" type="button">Start</button>
        </div>
      </div>
    </div>

    <div class="tetris-col tetris-col--right">
      <div class="tetris-next">
        <span class="tetris-next__label">Next</span>
        <canvas id="t-next" width="112" height="112"></canvas>
      </div>
      <div class="tetris-next">
        <span class="tetris-next__label">Hold</span>
        <canvas id="t-hold" width="112" height="112"></canvas>
      </div>
      <button class="tetris-btn tetris-btn--ghost" id="t-pause" type="button">Pause</button>
      <button class="tetris-btn tetris-btn--ghost" id="t-restart" type="button">Restart</button>
    </div>
  </div>

  <div class="tetris-touch-pad" aria-hidden="false">
    <button class="tetris-touch" data-action="left"   type="button" aria-label="Move left">◀</button>
    <button class="tetris-touch" data-action="rotate" type="button" aria-label="Rotate">↻</button>
    <button class="tetris-touch" data-action="right"  type="button" aria-label="Move right">▶</button>
    <button class="tetris-touch" data-action="soft"   type="button" aria-label="Soft drop">▼</button>
    <button class="tetris-touch" data-action="hard"   type="button" aria-label="Hard drop">⏬</button>
  </div>
</div>

<p class="tetris-hint">Tip: collect 4 lines at once for a Tetris (800 pts). Game speeds up every 10 lines. Press <kbd>C</kbd> to hold a piece.</p>

<script>
(function(){
  'use strict';
  var COLS = 10, ROWS = 20;
  var SHAPES = {
    I: [[0,0,0,0],[1,1,1,1],[0,0,0,0],[0,0,0,0]],
    O: [[1,1],[1,1]],
    T: [[0,1,0],[1,1,1],[0,0,0]],
    S: [[0,1,1],[1,1,0],[0,0,0]],
    Z: [[1,1,0],[0,1,1],[0,0,0]],
    L: [[0,0,1],[1,1,1],[0,0,0]],
    J: [[1,0,0],[1,1,1],[0,0,0]]
  };
  var COLORS = {
    I: '#27c4d6', O: '#e8b431', T: '#a04bc7',
    S: '#3fae5a', Z: '#d94545', L: '#e08a39', J: '#3a6dc7'
  };
  var TYPES = ['I','O','T','S','Z','L','J'];

  var board = document.getElementById('t-board');
  if (!board) return;
  var ctx = board.getContext('2d');
  var nextCanvas = document.getElementById('t-next');
  var nextCtx = nextCanvas.getContext('2d');
  var holdCanvas = document.getElementById('t-hold');
  var holdCtx = holdCanvas.getContext('2d');

  var cell = board.width / COLS;
  var grid, piece, nextType, holdType, canHold;
  var score, lines, level, best;
  var running, paused, gameOver;
  var dropAcc, dropInterval, lastTime;
  var bag = [];
  var animId = null;

  var $score = document.getElementById('t-score');
  var $lines = document.getElementById('t-lines');
  var $level = document.getElementById('t-level');
  var $best = document.getElementById('t-best');
  var $overlay = document.getElementById('t-overlay');
  var $overlayTitle = document.getElementById('t-overlay-title');
  var $overlayMsg = document.getElementById('t-overlay-msg');
  var $start = document.getElementById('t-start');
  var $pause = document.getElementById('t-pause');
  var $restart = document.getElementById('t-restart');

  function readBest(){
    try { return parseInt(localStorage.getItem('tetris_best') || '0', 10) || 0; }
    catch(e){ return 0; }
  }
  function writeBest(v){
    try { localStorage.setItem('tetris_best', String(v)); } catch(e){}
  }

  function refillBag(){
    bag = TYPES.slice();
    for (var i = bag.length - 1; i > 0; i--){
      var j = Math.floor(Math.random() * (i + 1));
      var t = bag[i]; bag[i] = bag[j]; bag[j] = t;
    }
  }
  function drawType(){
    if (bag.length === 0) refillBag();
    return bag.pop();
  }

  function newPiece(type){
    var shape = SHAPES[type].map(function(r){ return r.slice(); });
    return {
      type: type,
      shape: shape,
      x: Math.floor((COLS - shape[0].length) / 2),
      y: type === 'I' ? -1 : 0
    };
  }

  function collides(p, dx, dy, shape){
    shape = shape || p.shape;
    for (var y = 0; y < shape.length; y++){
      for (var x = 0; x < shape[y].length; x++){
        if (!shape[y][x]) continue;
        var nx = p.x + x + dx;
        var ny = p.y + y + dy;
        if (nx < 0 || nx >= COLS || ny >= ROWS) return true;
        if (ny >= 0 && grid[ny][nx]) return true;
      }
    }
    return false;
  }

  function rotateShape(shape){
    var n = shape.length;
    var out = [];
    for (var y = 0; y < n; y++){
      out.push([]);
      for (var x = 0; x < n; x++) out[y].push(shape[n - 1 - x][y]);
    }
    return out;
  }

  function tryRotate(){
    if (piece.type === 'O') return;
    var rotated = rotateShape(piece.shape);
    var kicks = [0, -1, 1, -2, 2];
    for (var i = 0; i < kicks.length; i++){
      if (!collides(piece, kicks[i], 0, rotated)){
        piece.shape = rotated;
        piece.x += kicks[i];
        return;
      }
    }
  }

  function lockPiece(){
    var s = piece.shape;
    for (var y = 0; y < s.length; y++){
      for (var x = 0; x < s[y].length; x++){
        if (!s[y][x]) continue;
        var ny = piece.y + y;
        var nx = piece.x + x;
        if (ny >= 0 && ny < ROWS && nx >= 0 && nx < COLS) grid[ny][nx] = piece.type;
      }
    }
    clearLines();
    spawn();
  }

  function clearLines(){
    var cleared = 0;
    for (var y = ROWS - 1; y >= 0; y--){
      var full = true;
      for (var x = 0; x < COLS; x++){
        if (!grid[y][x]){ full = false; break; }
      }
      if (full){
        grid.splice(y, 1);
        grid.unshift(new Array(COLS).fill(null));
        cleared++;
        y++;
      }
    }
    if (cleared > 0){
      var pts = [0, 100, 300, 500, 800][cleared] * level;
      score += pts;
      lines += cleared;
      var newLevel = Math.floor(lines / 10) + 1;
      if (newLevel !== level){
        level = newLevel;
        dropInterval = Math.max(80, 1000 - (level - 1) * 80);
      }
      updateHud();
    }
  }

  function softDrop(){
    if (!collides(piece, 0, 1)){ piece.y++; score++; updateHud(); }
    else { lockPiece(); }
  }
  function hardDrop(){
    var d = 0;
    while (!collides(piece, 0, 1)){ piece.y++; d++; }
    score += d * 2;
    updateHud();
    lockPiece();
  }
  function move(dx){
    if (!collides(piece, dx, 0)) piece.x += dx;
  }

  function holdPiece(){
    if (!canHold) return;
    var temp = piece.type;
    if (holdType){
      piece = newPiece(holdType);
    } else {
      piece = newPiece(nextType);
      nextType = drawType();
    }
    holdType = temp;
    canHold = false;
    if (collides(piece, 0, 0)){ doGameOver(); }
  }

  function spawn(){
    piece = newPiece(nextType);
    nextType = drawType();
    canHold = true;
    if (collides(piece, 0, 0)) doGameOver();
  }

  function doGameOver(){
    gameOver = true;
    running = false;
    if (score > best){ best = score; writeBest(best); $best.textContent = best; }
    $overlayTitle.textContent = 'Game Over';
    $overlayMsg.innerHTML = 'Final score: <strong>' + score + '</strong>';
    $start.textContent = 'Play Again';
    $overlay.classList.add('is-shown');
  }

  function updateHud(){
    $score.textContent = score;
    $lines.textContent = lines;
    $level.textContent = level;
  }

  function reset(){
    grid = [];
    for (var y = 0; y < ROWS; y++) grid.push(new Array(COLS).fill(null));
    bag = [];
    refillBag();
    nextType = drawType();
    spawn();
    score = 0; lines = 0; level = 1;
    dropInterval = 1000; dropAcc = 0; lastTime = 0;
    holdType = null; canHold = true;
    running = true; paused = false; gameOver = false;
    $overlay.classList.remove('is-shown');
    updateHud();
  }

  function drawCell(c, x, y, type, ghost){
    var color = COLORS[type] || '#888';
    var px = x * cell, py = y * cell;
    if (ghost){
      c.fillStyle = 'rgba(255,255,255,0.06)';
      c.fillRect(px + 1, py + 1, cell - 2, cell - 2);
      c.strokeStyle = color;
      c.globalAlpha = 0.6;
      c.lineWidth = 1.5;
      c.strokeRect(px + 1.5, py + 1.5, cell - 3, cell - 3);
      c.globalAlpha = 1;
      return;
    }
    var grad = c.createLinearGradient(px, py, px, py + cell);
    grad.addColorStop(0, lighten(color, 0.25));
    grad.addColorStop(1, color);
    c.fillStyle = grad;
    c.fillRect(px + 0.5, py + 0.5, cell - 1, cell - 1);
    c.strokeStyle = 'rgba(255,255,255,0.18)';
    c.lineWidth = 1;
    c.strokeRect(px + 1.5, py + 1.5, cell - 3, cell - 3);
  }

  function lighten(hex, amt){
    var num = parseInt(hex.slice(1), 16);
    var r = Math.min(255, ((num >> 16) & 255) + Math.round(255 * amt));
    var g = Math.min(255, ((num >> 8) & 255) + Math.round(255 * amt));
    var b = Math.min(255, (num & 255) + Math.round(255 * amt));
    return 'rgb(' + r + ',' + g + ',' + b + ')';
  }

  function ghostY(){
    var d = 0;
    while (!collides(piece, 0, d + 1)) d++;
    return piece.y + d;
  }

  function drawBoard(){
    ctx.clearRect(0, 0, board.width, board.height);
    /* faint grid */
    ctx.strokeStyle = 'rgba(255,255,255,0.04)';
    ctx.lineWidth = 1;
    for (var i = 1; i < COLS; i++){
      ctx.beginPath(); ctx.moveTo(i * cell, 0); ctx.lineTo(i * cell, board.height); ctx.stroke();
    }
    for (var j = 1; j < ROWS; j++){
      ctx.beginPath(); ctx.moveTo(0, j * cell); ctx.lineTo(board.width, j * cell); ctx.stroke();
    }
    /* settled cells */
    for (var y = 0; y < ROWS; y++){
      for (var x = 0; x < COLS; x++){
        if (grid[y][x]) drawCell(ctx, x, y, grid[y][x]);
      }
    }
    if (piece && !gameOver){
      /* ghost */
      var gy = ghostY();
      for (var py = 0; py < piece.shape.length; py++){
        for (var px = 0; px < piece.shape[py].length; px++){
          if (piece.shape[py][px]) drawCell(ctx, piece.x + px, gy + py, piece.type, true);
        }
      }
      /* current */
      for (var ly = 0; ly < piece.shape.length; ly++){
        for (var lx = 0; lx < piece.shape[ly].length; lx++){
          if (piece.shape[ly][lx]){
            var ay = piece.y + ly;
            if (ay >= 0) drawCell(ctx, piece.x + lx, ay, piece.type);
          }
        }
      }
    }
  }

  function drawPreview(c, canvas, type){
    c.clearRect(0, 0, canvas.width, canvas.height);
    if (!type) return;
    var shape = SHAPES[type];
    var rows = shape.length, cols = shape[0].length;
    var trimX0 = cols, trimX1 = 0, trimY0 = rows, trimY1 = 0;
    for (var y = 0; y < rows; y++){
      for (var x = 0; x < cols; x++){
        if (shape[y][x]){
          if (x < trimX0) trimX0 = x;
          if (x > trimX1) trimX1 = x;
          if (y < trimY0) trimY0 = y;
          if (y > trimY1) trimY1 = y;
        }
      }
    }
    var w = trimX1 - trimX0 + 1, h = trimY1 - trimY0 + 1;
    var size = Math.min(canvas.width / w, canvas.height / h) * 0.7;
    var ox = (canvas.width - size * w) / 2;
    var oy = (canvas.height - size * h) / 2;
    var color = COLORS[type];
    for (var py = 0; py < h; py++){
      for (var px = 0; px < w; px++){
        if (shape[py + trimY0][px + trimX0]){
          var x0 = ox + px * size, y0 = oy + py * size;
          var grad = c.createLinearGradient(x0, y0, x0, y0 + size);
          grad.addColorStop(0, lighten(color, 0.25));
          grad.addColorStop(1, color);
          c.fillStyle = grad;
          c.fillRect(x0 + 0.5, y0 + 0.5, size - 1, size - 1);
          c.strokeStyle = 'rgba(255,255,255,0.18)';
          c.lineWidth = 1;
          c.strokeRect(x0 + 1.5, y0 + 1.5, size - 3, size - 3);
        }
      }
    }
  }

  function loop(time){
    animId = requestAnimationFrame(loop);
    if (!running || paused || gameOver) return;
    if (!lastTime) lastTime = time;
    var dt = time - lastTime;
    lastTime = time;
    dropAcc += dt;
    if (dropAcc >= dropInterval){
      dropAcc = 0;
      if (!collides(piece, 0, 1)) piece.y++;
      else lockPiece();
    }
    drawBoard();
    drawPreview(nextCtx, nextCanvas, nextType);
    drawPreview(holdCtx, holdCanvas, holdType);
  }

  function startGame(){
    reset();
    if (animId) cancelAnimationFrame(animId);
    animId = requestAnimationFrame(loop);
  }

  function togglePause(){
    if (!running || gameOver) return;
    paused = !paused;
    if (paused){
      $overlayTitle.textContent = 'Paused';
      $overlayMsg.textContent = 'Press P or Resume';
      $start.textContent = 'Resume';
      $overlay.classList.add('is-shown');
    } else {
      $overlay.classList.remove('is-shown');
      lastTime = 0;
    }
  }

  /* ---------- Inputs ---------- */
  function action(name){
    if (gameOver){ return; }
    if (!running){ startGame(); return; }
    if (paused){ togglePause(); return; }
    switch(name){
      case 'left':   move(-1); break;
      case 'right':  move(1); break;
      case 'rotate': tryRotate(); break;
      case 'soft':   softDrop(); break;
      case 'hard':   hardDrop(); break;
      case 'hold':   holdPiece(); break;
    }
    drawBoard();
  }

  document.addEventListener('keydown', function(e){
    /* don't hijack typing in inputs (none on this page, but defensive) */
    var tag = (e.target && e.target.tagName || '').toLowerCase();
    if (tag === 'input' || tag === 'textarea') return;
    var k = e.key;
    if (k === 'ArrowLeft' || k === 'a' || k === 'A'){ e.preventDefault(); action('left'); }
    else if (k === 'ArrowRight' || k === 'd' || k === 'D'){ e.preventDefault(); action('right'); }
    else if (k === 'ArrowUp' || k === 'w' || k === 'W' || k === 'x' || k === 'X'){ e.preventDefault(); action('rotate'); }
    else if (k === 'ArrowDown' || k === 's' || k === 'S'){ e.preventDefault(); action('soft'); }
    else if (k === ' '){ e.preventDefault();
      if (gameOver || !running){ startGame(); }
      else if (paused){ togglePause(); }
      else { action('hard'); }
    }
    else if (k === 'p' || k === 'P'){ e.preventDefault(); togglePause(); }
    else if (k === 'c' || k === 'C' || k === 'Shift'){ e.preventDefault(); action('hold'); }
    else if (k === 'r' || k === 'R'){ e.preventDefault(); startGame(); }
  });

  /* Touch buttons */
  Array.prototype.forEach.call(document.querySelectorAll('.tetris-touch'), function(btn){
    var act = btn.getAttribute('data-action');
    btn.addEventListener('click', function(e){ e.preventDefault(); action(act); });
    btn.addEventListener('touchstart', function(e){ e.preventDefault(); action(act); }, {passive:false});
  });

  $start.addEventListener('click', function(){
    if (gameOver || !running){ startGame(); }
    else if (paused){ togglePause(); }
  });
  $pause.addEventListener('click', togglePause);
  $restart.addEventListener('click', startGame);

  /* Initial state — splash */
  best = readBest();
  $best.textContent = best;
  grid = [];
  for (var y0 = 0; y0 < ROWS; y0++) grid.push(new Array(COLS).fill(null));
  drawBoard();
  $overlay.classList.add('is-shown');
})();
</script>
