<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>Vector Navigator — Nelson Calculus Ch. 6 & 7</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@400;500;600;700&family=Crimson+Pro:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet"/>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --sky:#06101f;--sky2:#0b1e38;
  --accent:#00c8f0;--gold:#ffc947;--green:#00e5a0;--red:#ff4060;
  --purple:#a78bfa;--border:rgba(0,200,240,0.22);
  --card:rgba(10,24,48,0.97);--muted:rgba(0,200,240,0.5);
  --font-hud:'Share Tech Mono',monospace;
  --font-body:'Rajdhani',sans-serif;
  --font-serif:'Crimson Pro',Georgia,serif;
}
html{scroll-behavior:smooth}
body{background:var(--sky);color:#ddeeff;font-family:var(--font-body);font-size:16px;line-height:1.5;min-height:100vh;overflow-x:hidden}
body::before{content:'';position:fixed;inset:0;background:
  radial-gradient(1px 1px at 12% 18%,rgba(255,255,255,0.6) 0%,transparent 100%),
  radial-gradient(1px 1px at 34% 7%,rgba(255,255,255,0.5) 0%,transparent 100%),
  radial-gradient(1px 1px at 55% 22%,rgba(255,255,255,0.4) 0%,transparent 100%),
  radial-gradient(1px 1px at 72% 5%,rgba(255,255,255,0.6) 0%,transparent 100%),
  radial-gradient(1px 1px at 88% 15%,rgba(255,255,255,0.5) 0%,transparent 100%),
  radial-gradient(1px 1px at 6% 45%,rgba(255,255,255,0.3) 0%,transparent 100%),
  radial-gradient(1px 1px at 92% 52%,rgba(255,255,255,0.4) 0%,transparent 100%),
  radial-gradient(1px 1px at 47% 62%,rgba(255,255,255,0.3) 0%,transparent 100%),
  radial-gradient(1px 1px at 22% 78%,rgba(255,255,255,0.4) 0%,transparent 100%),
  radial-gradient(1px 1px at 65% 88%,rgba(255,255,255,0.3) 0%,transparent 100%);
  pointer-events:none;z-index:0}
#game-root{position:relative;z-index:1;max-width:820px;margin:0 auto;padding:0 0 3rem}

/* HUD */
#hud{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;
  padding:12px 20px;background:rgba(0,0,0,0.65);border-bottom:1px solid var(--border);backdrop-filter:blur(6px)}
#hud-brand{font-family:var(--font-hud);font-size:13px;color:var(--gold);letter-spacing:3px;display:flex;align-items:center;gap:8px}
.hud-pill{font-family:var(--font-hud);font-size:11px;background:rgba(0,200,240,0.1);
  border:1px solid var(--border);border-radius:4px;padding:4px 12px;color:var(--accent);letter-spacing:1px}

/* Progress */
#progress-wrap{padding:14px 20px 0;display:flex;align-items:center;gap:12px}
#progress{display:flex;gap:6px}
.pdot{width:40px;height:6px;border-radius:3px;background:rgba(0,200,240,0.12);border:1px solid var(--border);transition:all .35s}
.pdot.done{background:var(--green);border-color:var(--green)}
.pdot.active{background:var(--accent);border-color:var(--accent);box-shadow:0 0 10px rgba(0,200,240,0.6)}
.pdot.wrong-dot{background:var(--red);border-color:var(--red)}
#progress-label{font-family:var(--font-hud);font-size:11px;color:var(--muted);letter-spacing:1px}

/* Level card */
.level-card{margin:14px 20px 0;background:var(--card);border:1px solid var(--border);
  border-radius:14px;padding:1.5rem 1.75rem;position:relative;overflow:hidden}
.level-card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;
  background:linear-gradient(90deg,var(--accent),var(--purple),var(--gold))}
.level-tag{font-family:var(--font-hud);font-size:10px;color:var(--accent);letter-spacing:3px;margin-bottom:8px}
.level-title{font-size:24px;font-weight:700;color:#fff;margin-bottom:10px;line-height:1.2}
.level-story{font-family:var(--font-serif);font-size:15px;color:#aac8e8;line-height:1.7;margin-bottom:14px}
.mission-box{background:rgba(0,200,240,0.06);border-left:3px solid var(--accent);
  border-radius:0 8px 8px 0;padding:10px 14px;font-size:14px;color:#cce4f6;line-height:1.6}
.mission-box strong{color:var(--accent);font-family:var(--font-hud);font-size:10px;display:block;margin-bottom:5px;letter-spacing:2px}

/* Canvas — tall and wide */
#canvas-wrap{position:relative;margin:14px 20px 0;border-radius:12px;overflow:hidden;border:1px solid var(--border)}
canvas#sim{display:block;width:100%;height:auto;background:var(--sky2)}
#canvas-badge{position:absolute;top:10px;left:12px;font-family:var(--font-hud);font-size:10px;color:var(--muted);letter-spacing:2px;background:rgba(0,0,0,0.5);padding:3px 8px;border-radius:4px}

/* Legend */
#legend{display:flex;gap:18px;flex-wrap:wrap;padding:8px 20px;font-family:var(--font-hud);font-size:11px}
.leg-item{display:flex;align-items:center;gap:7px;color:rgba(200,220,240,0.7)}
.leg-line{width:28px;height:3px;border-radius:2px}

/* Question */
#question-wrap{margin:14px 20px 0;background:var(--card);border:1px solid var(--border);border-radius:12px;padding:1.25rem 1.5rem}
.q-label{font-family:var(--font-hud);font-size:10px;color:var(--gold);letter-spacing:3px;margin-bottom:8px}
.q-stem{font-size:17px;color:#e8f4ff;margin-bottom:16px;line-height:1.65;font-family:var(--font-serif)}
.q-stem em{color:var(--gold);font-style:normal;font-weight:600;font-family:var(--font-body)}
.choices{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.choice-btn{background:rgba(0,200,240,0.04);border:1px solid var(--border);
  border-radius:9px;padding:12px 14px;color:#cce4f6;font-family:var(--font-body);
  font-size:15px;cursor:pointer;text-align:left;transition:all .18s;line-height:1.4}
.choice-btn:hover{background:rgba(0,200,240,0.13);border-color:var(--accent);color:#fff;transform:translateY(-1px)}
.choice-btn.correct{background:rgba(0,229,160,0.14);border-color:var(--green);color:var(--green);transform:none}
.choice-btn.wrong{background:rgba(255,64,96,0.1);border-color:var(--red);color:var(--red);transform:none}
.choice-btn:disabled{cursor:default;transform:none}

/* Hint */
#feedback{margin:10px 20px 0;border-radius:10px;font-size:14px;line-height:1.75;display:none;padding:14px 16px}
#feedback.hint-fb{background:rgba(255,201,71,0.08);border:1px solid rgba(255,201,71,0.35);color:#ffe8a0}
#feedback strong{display:block;font-family:var(--font-hud);font-size:11px;letter-spacing:2px;margin-bottom:6px}

/* Solution screen */
#solution-screen{display:none;margin:14px 20px 0;border-radius:14px;overflow:hidden;border:1px solid var(--border)}
.sol-header{padding:14px 18px 12px;display:flex;align-items:center;gap:12px}
.sol-header.correct-h{background:rgba(0,229,160,0.12);border-bottom:1px solid rgba(0,229,160,0.25)}
.sol-header.wrong-h{background:rgba(255,64,96,0.1);border-bottom:1px solid rgba(255,64,96,0.25)}
.sol-verdict{font-family:var(--font-hud);font-size:13px;letter-spacing:2px}
.sol-verdict.cv{color:var(--green)}
.sol-verdict.wv{color:var(--red)}
.sol-icon{font-size:22px}
.sol-body{background:var(--card);padding:16px 18px}
.sol-story{font-family:var(--font-serif);font-size:15px;color:#b8d8f0;line-height:1.75;margin-bottom:14px}
.sol-steps-label{font-family:var(--font-hud);font-size:10px;color:var(--purple);letter-spacing:3px;margin-bottom:8px}
.sol-steps{background:rgba(0,0,0,0.35);border-radius:8px;padding:12px 16px;
  font-family:var(--font-hud);font-size:12px;line-height:2.1;color:#c8e8ff;border:1px solid rgba(167,139,250,0.18)}
.step-line{display:flex;gap:10px;align-items:baseline}
.step-n{color:var(--purple);min-width:20px;font-size:11px}
.step-t{color:#c8e8ff}
.step-t em{color:var(--gold);font-style:normal}
.sol-answer-box{margin-top:12px;background:rgba(0,229,160,0.08);border:1px solid rgba(0,229,160,0.3);
  border-radius:8px;padding:10px 14px;font-family:var(--font-hud);font-size:13px;color:var(--green);text-align:center}

/* Nav */
#nav-wrap{display:flex;justify-content:flex-end;padding:12px 20px 0;gap:10px;flex-wrap:wrap}
.nav-btn{font-family:var(--font-hud);font-size:11px;letter-spacing:2px;padding:10px 22px;border-radius:8px;border:1px solid;cursor:pointer;transition:all .18s}
.nav-btn.primary{background:var(--accent);border-color:var(--accent);color:#001828;font-weight:700}
.nav-btn.primary:hover{background:#33d8ff;transform:translateY(-1px)}
.nav-btn.ghost{background:transparent;border-color:var(--border);color:var(--muted)}
.nav-btn.ghost:hover{border-color:var(--accent);color:var(--accent)}

/* Score */
#score-screen{display:none;padding:2.5rem 1.5rem;text-align:center;margin:0 20px}
.score-emblem{width:100px;height:100px;border-radius:50%;margin:0 auto 1rem;border:2px solid var(--gold);
  display:flex;align-items:center;justify-content:center;font-size:40px;background:rgba(255,201,71,0.1)}
.score-title{font-family:var(--font-hud);font-size:24px;color:var(--gold);letter-spacing:5px;margin-bottom:6px}
.score-num{font-size:72px;font-weight:700;color:var(--accent);line-height:1;font-family:var(--font-hud)}
.score-denom{font-family:var(--font-hud);font-size:14px;color:var(--muted);margin-bottom:1rem}
.score-msg{font-family:var(--font-serif);font-size:17px;color:#aac8e8;max-width:420px;margin:0 auto 1.5rem;line-height:1.7}
.score-concepts{background:var(--card);border:1px solid var(--border);border-radius:12px;
  padding:1.25rem;max-width:500px;margin:0 auto 1.5rem;text-align:left}
.score-concepts h4{font-family:var(--font-hud);font-size:10px;color:var(--purple);letter-spacing:3px;margin-bottom:10px}
.concept-row{display:flex;justify-content:space-between;align-items:center;
  padding:6px 0;border-bottom:1px solid rgba(255,255,255,0.05);font-size:14px;color:#cce4f6}
.concept-row:last-child{border-bottom:none}
.concept-status{font-family:var(--font-hud);font-size:12px}

@media(max-width:560px){.choices{grid-template-columns:1fr}}
</style>
</head>
<body>
<div id="game-root">

  <div id="hud">
    <div id="hud-brand"><span>✈</span> VECTOR NAVIGATOR</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap">
      <div class="hud-pill" id="hud-level">LEVEL 1 / 5</div>
      <div class="hud-pill" id="hud-score">SCORE: 0 / 100</div>
    </div>
  </div>

  <div id="progress-wrap">
    <div id="progress"></div>
    <div id="progress-label">MISSION PROGRESS</div>
  </div>

  <div id="main-view">
    <div class="level-card">
      <div class="level-tag" id="level-tag"></div>
      <div class="level-title" id="level-title"></div>
      <div class="level-story" id="level-story"></div>
      <div class="mission-box"><strong>📡 MISSION BRIEFING</strong><span id="mission-text"></span></div>
    </div>

    <div id="canvas-wrap">
      <canvas id="sim" width="780" height="420"></canvas>
      <div id="canvas-badge">VECTOR DIAGRAM</div>
    </div>

    <div id="legend">
      <div class="leg-item"><div class="leg-line" style="background:#00c8f0"></div>Plane velocity <b>p⃗</b></div>
      <div class="leg-item"><div class="leg-line" style="background:#ffc947"></div>Wind velocity <b>w⃗</b></div>
      <div class="leg-item"><div class="leg-line" style="background:#00e5a0"></div>Ground velocity <b>g⃗ = p⃗ + w⃗</b></div>
    </div>

    <div id="question-wrap">
      <div class="q-label">🎯 PILOT DECISION</div>
      <div class="q-stem" id="q-stem"></div>
      <div class="choices" id="choices"></div>
    </div>

    <div id="feedback"></div>

    <div id="solution-screen">
      <div class="sol-header" id="sol-header">
        <span class="sol-icon" id="sol-icon"></span>
        <span class="sol-verdict" id="sol-verdict"></span>
      </div>
      <div class="sol-body">
        <div class="sol-story" id="sol-story"></div>
        <div class="sol-steps-label">STEP-BY-STEP SOLUTION</div>
        <div class="sol-steps" id="sol-steps"></div>
        <div class="sol-answer-box" id="sol-answer"></div>
      </div>
    </div>

    <div id="nav-wrap">
      <button class="nav-btn ghost" id="btn-hint" onclick="showHint()">💡 HINT</button>
      <button class="nav-btn primary" id="btn-next" onclick="nextLevel()" style="display:none">NEXT LEVEL →</button>
    </div>
  </div>

  <div id="score-screen">
    <div class="score-emblem" id="score-emblem">🎖</div>
    <div class="score-title">MISSION DEBRIEF</div>
    <div class="score-num" id="final-score">0</div>
    <div class="score-denom">OUT OF 100 POINTS</div>
    <div class="score-msg" id="score-msg"></div>
    <div class="score-concepts" id="score-concepts"></div>
    <button class="nav-btn primary" onclick="restartGame()">↺ FLY AGAIN</button>
  </div>

</div>

<script>
function bearingToCanvas(b,s,sc){
  const r=b*Math.PI/180;
  return{dx:s*Math.sin(r)*sc, dy:-s*Math.cos(r)*sc};
}
function mag(x,y){return Math.sqrt(x*x+y*y);}
function bearingFromXY(dx,dy){
  let b=Math.atan2(dx,-dy)*180/Math.PI;
  if(b<0)b+=360;
  return Math.round(b);
}

const LEVELS=[
{
  tag:'LEVEL 1 — PARALLEL VECTORS',
  title:'Full Throttle: Tailwind Over Georgian Bay',
  story:'You are 45 minutes into your solo cross-country on a Cessna 172. A tailwind is blowing dead astern — same bearing as your heading. ATC needs your estimated ground speed to update your arrival time at Muskoka Airport.',
  mission:'Your heading: bearing 090°, airspeed 200 km/h. Tailwind: bearing 090°, speed 50 km/h.',
  planeBearing:90,planeSpd:200,windBearing:90,windSpd:50,
  question:'ATC radios: "Cessna Golf-Foxtrot, confirm ground speed for sequencing — do you have a tailwind advantage today?" What do you report back?',
  choices:['150 km/h — the tailwind is somehow slowing me down','200 km/h — wind has zero effect on ground speed','250 km/h — tailwind adds directly to my ground speed','300 km/h — we multiply speeds when they match directions'],
  correct:2,concept:'Parallel vector addition',
  hint:'Both vectors point bearing 090° — they are perfectly parallel. When vectors point in the same direction, simply add their magnitudes: |g⃗| = |p⃗| + |w⃗|.',
  solStory:'The tailwind is blowing exactly along your route — same bearing, same direction. In vector terms, both p⃗ and w⃗ point East. When two vectors are parallel, their resultant is the sum of their magnitudes. You will arrive early — the tailwind gave you a free 50 km/h boost.',
  solSteps:[
    {n:'1',t:'Both vectors on bearing 090° → they are <em>parallel</em>'},
    {n:'2',t:'g⃗ = p⃗ + w⃗ = (200, 0) + (50, 0) = <em>(250, 0) km/h</em>'},
    {n:'3',t:'|g⃗| = √(250² + 0²) = <em>250 km/h</em>'},
    {n:'4',t:'Direction of g⃗: bearing <em>090°</em> — same as before, just faster'},
  ],
  solAnswer:'✈  Ground speed = 250 km/h  |  Bearing 090°'
},
{
  tag:'LEVEL 2 — OPPOSITE VECTORS',
  title:'Fighting the Jet Stream: Ottawa to Montréal',
  story:'You are flying a Dash-8 regional service. A forecasted jet-stream headwind is hammering you head-on. Dispatch needs your true ground speed to calculate whether you carry enough fuel reserves for the leg.',
  mission:'Your heading: bearing 090°, airspeed 600 km/h. Headwind: bearing 270° (directly opposing), speed 80 km/h.',
  planeBearing:90,planeSpd:600,windBearing:270,windSpd:80,
  question:'Dispatch calls: "State ground speed for fuel planning — we show a headwind component on your route." You check your nav computer and respond:',
  choices:['680 km/h — I add the headwind to my airspeed','600 km/h — opposing wind cancels itself out perfectly','520 km/h — headwind directly reduces my ground speed','440 km/h — I subtract the wind from both directions'],
  correct:2,concept:'Antiparallel vector subtraction',
  hint:'Bearing 270° is exactly opposite bearing 090°. An opposing vector has a negative component. Add (600, 0) + (−80, 0) and find the magnitude.',
  solStory:'A headwind on bearing 270° is the exact opposite of your heading on 090°. When you add a vector pointing backward, it shrinks your resultant — like swimming upstream. Your ground speed drops to 520 km/h and your fuel burn per kilometre just increased.',
  solSteps:[
    {n:'1',t:'Wind bearing 270° is opposite plane bearing 090° → <em>antiparallel</em>'},
    {n:'2',t:'Convert wind to components: w⃗ = (−80, 0) km/h (negative = West)'},
    {n:'3',t:'g⃗ = (600, 0) + (−80, 0) = <em>(520, 0) km/h</em>'},
    {n:'4',t:'|g⃗| = <em>520 km/h</em>  |  bearing <em>090°</em> — direction unchanged'},
  ],
  solAnswer:'✈  Ground speed = 520 km/h  |  Headwind cost 80 km/h'
},
{
  tag:'LEVEL 3 — PERPENDICULAR VECTORS',
  title:'Crosswind on Final — Toronto Pearson Approach',
  story:'You are on final approach to Toronto Pearson. A strong crosswind is blowing perpendicular to your runway. Ground Control needs your actual ground speed over the threshold to calculate safe landing distance. You cannot simply add — the vectors are at right angles.',
  mission:'Your heading: bearing 000° (North), airspeed 300 km/h. Crosswind: bearing 090° (East), speed 400 km/h.',
  planeBearing:0,planeSpd:300,windBearing:90,windSpd:400,
  question:'Ground Control asks: "State ground speed for landing distance." The crosswind is perfectly perpendicular — your hand reaches for the Pythagorean theorem:',
  choices:['700 km/h — add both speeds like parallel vectors','350 km/h — average the two perpendicular speeds','500 km/h — √(300² + 400²) — Pythagorean theorem','450 km/h — subtract the smaller from the larger'],
  correct:2,concept:'Pythagorean theorem for perpendicular vectors',
  hint:'Perpendicular vectors form a right triangle. Ground velocity is the hypotenuse. |g⃗| = √(300² + 400²). Recognize the numbers — 3, 4, 5 scaled by 100!',
  solStory:'Perpendicular vectors form a right angle — you cannot add their speeds directly. The ground velocity is the hypotenuse of a right triangle. The 300-400-500 relationship is the 3-4-5 Pythagorean triple scaled by 100. Real pilots verify this with onboard computers every crosswind landing.',
  solSteps:[
    {n:'1',t:'p⃗ = (0, 300) [N]   w⃗ = (400, 0) [E]  → <em>perpendicular — 90° apart</em>'},
    {n:'2',t:'g⃗ = (0+400, 300+0) = <em>(400, 300) km/h</em>'},
    {n:'3',t:'|g⃗| = √(400² + 300²) = √(160 000 + 90 000) = √250 000 = <em>500 km/h</em>'},
    {n:'4',t:'Bearing: arctan(400 / 300) ≈ arctan(1.33) ≈ <em>053°</em> — drifting East of North'},
  ],
  solAnswer:'✈  Ground speed = 500 km/h  |  Bearing 053°  (3-4-5 triangle × 100)'
},
{
  tag:'LEVEL 4 — COMPONENT DECOMPOSITION',
  title:'Overnight Freight — Angled Wind Over Lake Superior',
  story:'You are flying cargo overnight across Lake Superior. Weather reports a wind on bearing 060° — a northeast angle not aligned with any axis. You must break the wind into North and East parts using trigonometry, then add them to your velocity to find the true ground track.',
  mission:'Your heading: bearing 000° (North), airspeed 500 km/h. Wind: bearing 060°, speed 200 km/h.',
  planeBearing:0,planeSpd:500,windBearing:60,windSpd:200,
  question:'Your manual nav calculation is complete. You have decomposed the angled wind using sin and cos, added components, and applied Pythagoras. What ground speed do you log?',
  choices:['700 km/h — I added all three speeds together','580 km/h — I only counted the North part of the wind','624 km/h — I decomposed wind with sin/cos, then used Pythagoras','548 km/h — I subtracted the East component from the total'],
  correct:2,concept:'Component decomposition with trigonometry',
  hint:'Bearing 060°: East component = 200 × sin(60°) ≈ 173 km/h,  North component = 200 × cos(60°) = 100 km/h. Add to plane, then |g⃗| = √(gₓ² + gᵧ²).',
  solStory:'An angled wind cannot be handled as a simple number — you must decompose it into its East and North parts using the bearing formulas before adding. This is the core skill of vector navigation at the Nelson Chapter 7 level. sin gives the East component, cos gives the North.',
  solSteps:[
    {n:'1',t:'Wind: bearing 060°, 200 km/h → decompose using true-bearing formulas:'},
    {n:'2',t:'wₓ = 200 · sin(60°) = 200 · 0.866 ≈ <em>173 km/h [East]</em>'},
    {n:'3',t:'wᵧ = 200 · cos(60°) = 200 · 0.500 = <em>100 km/h [North]</em>'},
    {n:'4',t:'Plane p⃗ = (0, 500)  →  g⃗ = (0+173, 500+100) = <em>(173, 600)</em>'},
    {n:'5',t:'|g⃗| = √(173² + 600²) = √(29 929 + 360 000) = √389 929 ≈ <em>624 km/h</em>'},
    {n:'6',t:'Bearing: arctan(173 / 600) ≈ <em>016°</em> — slightly East of North'},
  ],
  solAnswer:'✈  Ground speed ≈ 624 km/h  |  Bearing 016°'
},
{
  tag:'LEVEL 5 — COURSE CORRECTION',
  title:'True Track: Vancouver to Calgary — The Crab Angle',
  story:'You must hold a ground track of exactly bearing 090° from Vancouver to Calgary. A northward wind keeps pushing your nose off course toward Edmonton. Fly bearing 090° and you will miss Calgary. You must aim slightly south — this is called the crab angle, used by every commercial pilot every day.',
  mission:'Required ground track: bearing 090°. Wind: bearing 000° (North), speed 100 km/h. Your airspeed: 500 km/h.',
  planeBearing:101.5,planeSpd:500,windBearing:0,windSpd:100,
  question:'ATC clears you "direct Calgary, maintain track 090°." The northward crosswind will push you off-route unless you compensate. Which heading do you dial into your HSI (Horizontal Situation Indicator)?',
  choices:['Heading 090° — fly straight East, the wind will sort itself out','Heading 079° — angle slightly North to match the wind direction','Heading 101° — angle slightly South so wind pushes me back onto track','Heading 180° — fly South until conditions improve'],
  correct:2,concept:'Crab angle and course correction',
  hint:'Wind pushes North (+y direction). To cancel it, your plane must contribute −100 in the North/South direction. For bearing α: pᵧ = 500·cos(α). Set pᵧ = −100 and solve for α.',
  solStory:'The crab angle is the difference between your heading and your actual ground track. Wind pushes you North, so you must point your nose South of your destination. Your plane\'s southward component exactly cancels the wind\'s northward push, and the resultant ground velocity points due East — bearing 090° — straight to Calgary.',
  solSteps:[
    {n:'1',t:'Need ground track bearing 090° → g⃗ must have <em>zero North/South component</em>'},
    {n:'2',t:'Wind w⃗ = (0, 100) km/h [North] — pushes everything northward'},
    {n:'3',t:'Plane must contribute −100 southward to cancel: pᵧ = 500 · cos(α) = −100'},
    {n:'4',t:'cos(α) = −100 / 500 = −0.200  →  α = arccos(−0.2) ≈ <em>101.5°</em>'},
    {n:'5',t:'Fly heading <em>bearing 101°</em> — nose slightly South of East'},
    {n:'6',t:'Resultant g⃗ points exactly bearing <em>090°</em>  ✓  You reach Calgary'},
  ],
  solAnswer:'✈  Fly heading 101° to maintain ground track bearing 090°'
}
];

let lvl=0,score=0,answered=false,results=[];
const C=document.getElementById('sim');
const ctx=C.getContext('2d');
const W=780,H=420;

function buildProgress(){
  const p=document.getElementById('progress');p.innerHTML='';
  LEVELS.forEach((_,i)=>{
    const d=document.createElement('div');
    if(results[i]===true)d.className='pdot done';
    else if(results[i]===false)d.className='pdot wrong-dot';
    else if(i===lvl)d.className='pdot active';
    else d.className='pdot';
    p.appendChild(d);
  });
}

function loadLevel(){
  answered=false;
  const L=LEVELS[lvl];
  document.getElementById('hud-level').textContent=`LEVEL ${lvl+1} / ${LEVELS.length}`;
  document.getElementById('hud-score').textContent=`SCORE: ${score} / 100`;
  document.getElementById('level-tag').textContent=L.tag;
  document.getElementById('level-title').textContent=L.title;
  document.getElementById('level-story').textContent=L.story;
  document.getElementById('mission-text').textContent=L.mission;
  document.getElementById('q-stem').innerHTML=L.question;
  document.getElementById('feedback').style.display='none';
  document.getElementById('solution-screen').style.display='none';
  document.getElementById('btn-next').style.display='none';
  document.getElementById('btn-hint').style.display='inline-block';
  const ch=document.getElementById('choices');ch.innerHTML='';
  L.choices.forEach((c,i)=>{
    const b=document.createElement('button');
    b.className='choice-btn';b.textContent=c;b.onclick=()=>answer(i,b);
    ch.appendChild(b);
  });
  buildProgress();
  drawScene(L);
}

function drawScene(L){
  ctx.clearRect(0,0,W,H);
  const grad=ctx.createLinearGradient(0,0,0,H);
  grad.addColorStop(0,'#040c1a');grad.addColorStop(1,'#0b1e38');
  ctx.fillStyle=grad;ctx.fillRect(0,0,W,H);
  ctx.strokeStyle='rgba(0,200,240,0.055)';ctx.lineWidth=0.8;
  for(let x=0;x<W;x+=50){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke();}
  for(let y=0;y<H;y+=50){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke();}
  drawCompass(W-75,75,58);

  // auto-scale
  const pv1=bearingToCanvas(L.planeBearing,L.planeSpd,1);
  const wv1=bearingToCanvas(L.windBearing,L.windSpd,1);
  const g1x=pv1.dx+wv1.dx,g1y=pv1.dy+wv1.dy;
  const pts=[[0,0],[pv1.dx,pv1.dy],[pv1.dx+wv1.dx,pv1.dy+wv1.dy],[g1x,g1y]];
  const allX=pts.map(p=>p[0]),allY=pts.map(p=>p[1]);
  const spanX=(Math.max(...allX)-Math.min(...allX))||100;
  const spanY=(Math.max(...allY)-Math.min(...allY))||100;
  const pad=150;
  const sc=Math.min((W-pad*2)/spanX,(H-pad*2)/spanY,0.42);
  const scale=Math.max(0.07,Math.min(sc,0.42));

  const ox=W*0.26,oy=H*0.54;
  const pv=bearingToCanvas(L.planeBearing,L.planeSpd,scale);
  const wv=bearingToCanvas(L.windBearing,L.windSpd,scale);
  const gx=pv.dx+wv.dx,gy=pv.dy+wv.dy;

  // destination
  const sx=ox+gx*1.08,sy=oy+gy*1.08;
  drawStar(sx,sy,11,'#ffc947');
  ctx.font='bold 12px "Share Tech Mono"';ctx.fillStyle='#ffc947';
  ctx.textAlign='left';ctx.textBaseline='middle';
  ctx.fillText('DESTINATION',sx+15,sy);

  // dashed parallelogram
  ctx.save();ctx.strokeStyle='rgba(255,255,255,0.07)';ctx.lineWidth=1;ctx.setLineDash([5,5]);
  ctx.beginPath();ctx.moveTo(ox+pv.dx,oy+pv.dy);ctx.lineTo(ox+pv.dx+wv.dx,oy+pv.dy+wv.dy);
  ctx.moveTo(ox+wv.dx,oy+wv.dy);ctx.lineTo(ox+wv.dx+gx,oy+wv.dy+gy);
  ctx.stroke();ctx.setLineDash([]);ctx.restore();

  drawArrow(ox,oy,ox+pv.dx,oy+pv.dy,'#00c8f0',3.5,
    'p\u20d7',`${L.planeSpd} km/h`,`brg ${String(Math.round(L.planeBearing)).padStart(3,'0')}\u00b0`);
  drawArrow(ox+pv.dx,oy+pv.dy,ox+pv.dx+wv.dx,oy+pv.dy+wv.dy,'#ffc947',3,
    'w\u20d7',`${L.windSpd} km/h`,`brg ${String(Math.round(L.windBearing)).padStart(3,'0')}\u00b0`);
  const gmag=Math.round(mag(gx,gy)/scale);
  const gbrg=bearingFromXY(gx,gy);
  drawArrow(ox,oy,ox+gx,oy+gy,'#00e5a0',4.5,
    'g\u20d7',`${gmag} km/h`,`brg ${String(gbrg).padStart(3,'0')}\u00b0`);

  ctx.fillStyle='rgba(0,200,240,0.28)';ctx.beginPath();ctx.arc(ox,oy,10,0,Math.PI*2);ctx.fill();
  ctx.fillStyle='#fff';ctx.beginPath();ctx.arc(ox,oy,4,0,Math.PI*2);ctx.fill();
  ctx.font='24px serif';ctx.textAlign='left';ctx.fillText('\u2708',ox-13,oy+8);

  // component box
  const pxK=Math.round(L.planeSpd*Math.sin(L.planeBearing*Math.PI/180));
  const pyK=Math.round(L.planeSpd*Math.cos(L.planeBearing*Math.PI/180));
  const wxK=Math.round(L.windSpd*Math.sin(L.windBearing*Math.PI/180));
  const wyK=Math.round(L.windSpd*Math.cos(L.windBearing*Math.PI/180));
  const gxK=pxK+wxK,gyK=pyK+wyK;
  ctx.save();
  ctx.fillStyle='rgba(0,0,0,0.62)';
  rr(ctx,12,H-100,236,88,8);ctx.fill();
  ctx.strokeStyle='rgba(0,200,240,0.2)';ctx.lineWidth=1;
  rr(ctx,12,H-100,236,88,8);ctx.stroke();
  ctx.font='9px "Share Tech Mono"';ctx.fillStyle='rgba(0,200,240,0.7)';
  ctx.textAlign='left';ctx.textBaseline='top';
  ctx.fillText('COMPONENT FORM  (East, North)',20,H-88);
  ctx.font='12px "Share Tech Mono"';
  ctx.fillStyle='#80d8ff';ctx.fillText(`p\u20d7 = (${pxK},  ${pyK}) km/h`,20,H-72);
  ctx.fillStyle='#ffe082';ctx.fillText(`w\u20d7 = (${wxK},  ${wyK}) km/h`,20,H-53);
  ctx.fillStyle='#80ffcc';ctx.fillText(`g\u20d7 = (${gxK},  ${gyK}) km/h`,20,H-34);
  ctx.restore();
}

function rr(c,x,y,w,h,r){
  c.beginPath();c.moveTo(x+r,y);c.lineTo(x+w-r,y);c.arcTo(x+w,y,x+w,y+r,r);
  c.lineTo(x+w,y+h-r);c.arcTo(x+w,y+h,x+w-r,y+h,r);
  c.lineTo(x+r,y+h);c.arcTo(x,y+h,x,y+h-r,r);
  c.lineTo(x,y+r);c.arcTo(x,y,x+r,y,r);c.closePath();
}

function drawCompass(cx,cy,r){
  ctx.save();
  ctx.fillStyle='rgba(0,0,0,0.52)';ctx.beginPath();ctx.arc(cx,cy,r,0,Math.PI*2);ctx.fill();
  ctx.strokeStyle='rgba(0,200,240,0.3)';ctx.lineWidth=1;ctx.beginPath();ctx.arc(cx,cy,r,0,Math.PI*2);ctx.stroke();
  for(let i=0;i<8;i++){
    const a=i*Math.PI/4,inner=i%2===0?r-10:r-6;
    ctx.strokeStyle=i%2===0?'rgba(0,200,240,0.6)':'rgba(0,200,240,0.3)';
    ctx.lineWidth=i%2===0?1.5:1;
    ctx.beginPath();ctx.moveTo(cx+inner*Math.sin(a),cy-inner*Math.cos(a));
    ctx.lineTo(cx+r*Math.sin(a),cy-r*Math.cos(a));ctx.stroke();
  }
  [['N',0],['E',90],['S',180],['W',270]].forEach(([d,b])=>{
    const rad=b*Math.PI/180,tx=cx+(r-18)*Math.sin(rad),ty=cy-(r-18)*Math.cos(rad);
    ctx.font='bold 12px "Share Tech Mono"';
    ctx.fillStyle=d==='N'?'#ff7070':'rgba(0,200,240,0.9)';
    ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText(d,tx,ty);
  });
  ctx.restore();
}

function drawArrow(x1,y1,x2,y2,color,lw,name,speed,bearing){
  const dx=x2-x1,dy=y2-y1,len=Math.sqrt(dx*dx+dy*dy);
  if(len<4)return;
  const ang=Math.atan2(dy,dx);
  ctx.save();
  // glow
  ctx.strokeStyle=color;ctx.lineWidth=lw+5;ctx.globalAlpha=0.15;
  ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
  ctx.globalAlpha=1;
  ctx.strokeStyle=color;ctx.lineWidth=lw;
  ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
  // arrowhead
  ctx.fillStyle=color;ctx.translate(x2,y2);ctx.rotate(ang);
  ctx.beginPath();ctx.moveTo(0,0);ctx.lineTo(-18,-7);ctx.lineTo(-18,7);ctx.closePath();ctx.fill();
  ctx.restore();
  // label — pick the side further from canvas centre
  const mx=(x1+x2)/2,my=(y1+y2)/2,perp=ang+Math.PI/2,off=30;
  const lx1=mx+off*Math.cos(perp),ly1=my+off*Math.sin(perp);
  const lx2=mx-off*Math.cos(perp),ly2=my-off*Math.sin(perp);
  const d1=Math.sqrt((lx1-W/2)**2+(ly1-H/2)**2);
  const d2=Math.sqrt((lx2-W/2)**2+(ly2-H/2)**2);
  const lx=d1>d2?lx1:lx2,ly=d1>d2?ly1:ly2;
  const bw=148,bh=50;
  ctx.save();
  ctx.fillStyle='rgba(0,0,0,0.75)';
  rr(ctx,lx-bw/2,ly-bh/2,bw,bh,7);ctx.fill();
  ctx.strokeStyle=color+'55';ctx.lineWidth=1;
  rr(ctx,lx-bw/2,ly-bh/2,bw,bh,7);ctx.stroke();
  ctx.textAlign='center';ctx.textBaseline='middle';
  ctx.font='bold 14px "Share Tech Mono"';ctx.fillStyle=color;
  ctx.fillText(`${name} = ${speed}`,lx,ly-8);
  ctx.font='11px "Share Tech Mono"';ctx.fillStyle='rgba(200,220,240,0.6)';
  ctx.fillText(bearing,lx,ly+10);
  ctx.restore();
}

function drawStar(cx,cy,r,color){
  ctx.save();ctx.fillStyle=color;ctx.shadowColor=color;ctx.shadowBlur=16;
  ctx.beginPath();
  for(let i=0;i<5;i++){
    const a=i*4*Math.PI/5-Math.PI/2,b=a+2*Math.PI/5;
    if(i===0)ctx.moveTo(cx+r*Math.cos(a),cy+r*Math.sin(a));
    else ctx.lineTo(cx+r*Math.cos(a),cy+r*Math.sin(a));
    ctx.lineTo(cx+r*0.42*Math.cos(b),cy+r*0.42*Math.sin(b));
  }
  ctx.closePath();ctx.fill();ctx.restore();
}

function answer(idx,btn){
  if(answered)return;
  answered=true;
  const L=LEVELS[lvl];
  document.querySelectorAll('.choice-btn').forEach(b=>b.disabled=true);
  const correct=(idx===L.correct);
  if(correct){btn.className='choice-btn correct';score+=20;results[lvl]=true;}
  else{
    btn.className='choice-btn wrong';
    document.querySelectorAll('.choice-btn')[L.correct].className='choice-btn correct';
    results[lvl]=false;
  }
  document.getElementById('hud-score').textContent=`SCORE: ${score} / 100`;
  document.getElementById('btn-hint').style.display='none';
  document.getElementById('btn-next').style.display='inline-block';
  buildProgress();
  showSolution(L,correct);
}

function showSolution(L,wasCorrect){
  const ss=document.getElementById('solution-screen');
  ss.style.display='block';
  const hdr=document.getElementById('sol-header');
  hdr.className='sol-header '+(wasCorrect?'correct-h':'wrong-h');
  document.getElementById('sol-icon').textContent=wasCorrect?'✅':'❌';
  const vd=document.getElementById('sol-verdict');
  vd.className='sol-verdict '+(wasCorrect?'cv':'wv');
  vd.textContent=wasCorrect?'CORRECT — NICE FLYING!':'INCORRECT — HERE\'S HOW IT WORKS';
  document.getElementById('sol-story').textContent=L.solStory;
  document.getElementById('sol-answer').textContent=L.solAnswer;
  document.getElementById('sol-steps').innerHTML=L.solSteps.map(s=>
    `<div class="step-line"><span class="step-n">${s.n}.</span><span class="step-t">${s.t}</span></div>`
  ).join('');
  setTimeout(()=>ss.scrollIntoView({behavior:'smooth',block:'nearest'}),80);
}

function showHint(){
  const fb=document.getElementById('feedback');
  fb.className='hint-fb';fb.style.display='block';
  fb.innerHTML=`<strong>💡 PILOT HINT</strong>${LEVELS[lvl].hint}`;
}

function nextLevel(){
  lvl++;
  if(lvl>=LEVELS.length){endGame();return;}
  loadLevel();
  window.scrollTo({top:0,behavior:'smooth'});
}

function endGame(){
  document.getElementById('main-view').style.display='none';
  document.getElementById('score-screen').style.display='block';
  document.getElementById('final-score').textContent=score;
  const pct=score/100;
  document.getElementById('score-emblem').textContent=['😐','🙂','😊','🌟','🎖'][Math.min(4,Math.floor(pct*5))];
  const msgs={
    100:'Perfect score! You have mastered vector navigation with true bearings — parallel, antiparallel, perpendicular, trig decomposition, and crab angles. Nelson Ch.6–7 exam? No problem.',
    80:'Excellent. Strong grasp of vectors and bearings. Review the one you missed and you are exam-ready.',
    60:'Good progress. Focus on the step-by-step solutions for missed levels — especially component decomposition using sin and cos.',
    0:'Vectors take practice. Re-read each solution carefully and use the Hint on your next run.'
  };
  document.getElementById('score-msg').textContent=msgs[pct===1?100:score>=80?80:score>=60?60:0];
  document.getElementById('score-concepts').innerHTML='<h4>CONCEPTS COVERED</h4>'+
    LEVELS.map((L,i)=>`<div class="concept-row"><span>${L.concept}</span>
      <span class="concept-status" style="color:${results[i]?'var(--green)':'var(--red)'}">
      ${results[i]?'✓ CORRECT':'✗ MISSED'}</span></div>`).join('');
}

function restartGame(){
  lvl=0;score=0;results=[];answered=false;
  document.getElementById('score-screen').style.display='none';
  document.getElementById('main-view').style.display='block';
  loadLevel();window.scrollTo({top:0,behavior:'smooth'});
}

loadLevel();
</script>
</body>
</html>
