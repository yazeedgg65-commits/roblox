<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
<title>BlockWorld</title>
<style>
*{box-sizing:border-box}
html,body{margin:0;height:100%;overflow:hidden;font-family:system-ui,sans-serif;background:#87ceeb;user-select:none}
canvas{display:block;touch-action:none}
#menu{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(20,24,40,.85);z-index:10}
.card{background:#fff;padding:28px;border-radius:16px;width:300px;text-align:center}
.card h1{margin:0 0 16px}
.card input[type=text]{width:100%;padding:10px;font-size:16px;border:2px solid #ddd;border-radius:8px;margin-bottom:10px;text-align:center}
.card input[type=color]{width:100%;height:40px;border:none;margin-bottom:12px;background:none}
.card button{width:100%;padding:12px;font-size:18px;font-weight:700;border:0;border-radius:10px;background:#00b06f;color:#fff;cursor:pointer}
#hud{position:fixed;top:10px;left:10px;color:#fff;font-weight:700;font-size:18px;line-height:1.6;text-shadow:0 1px 3px #000;pointer-events:none}
#toast{position:fixed;top:22%;left:0;right:0;text-align:center;color:#ffe650;font-size:34px;font-weight:900;text-shadow:0 2px 4px #000;opacity:0;transition:opacity .5s;pointer-events:none}
#chat{position:fixed;left:10px;bottom:10px;width:min(320px,60vw)}
#log{max-height:150px;overflow:hidden;color:#fff;font-size:14px;text-shadow:0 1px 2px #000;margin-bottom:6px}
#log div{background:rgba(0,0,0,.35);padding:2px 8px;border-radius:6px;margin-top:2px}
#msg{width:100%;padding:8px;border-radius:8px;border:0;background:rgba(255,255,255,.85)}
#jump{position:fixed;right:24px;bottom:30px;width:76px;height:76px;border-radius:50%;border:0;background:rgba(255,255,255,.6);font-size:16px;font-weight:700}
@media (hover:hover){#jump{display:none}}
</style>
<script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js"}}</script>
</head>
<body>
<div id="menu"><div class="card">
  <h1>BlockWorld</h1>
  <input id="name" type="text" maxlength="16" placeholder="اسمك">
  <input id="color" type="color" value="#3a86ff">
  <button id="play">العب</button>
</div></div>
<div id="hud"></div>
<div id="toast"></div>
<div id="chat"><div id="log"></div><input id="msg" maxlength="100" placeholder="اضغط Enter للكتابة" dir="auto"></div>
<button id="jump">قفز</button>

<script type="module">
import * as THREE from 'three';

const $ = id => document.getElementById(id);
const clamp = (v, a, b) => Math.max(a, Math.min(b, v));
const R = 0.6, H = 5.2, STAGES = 20;

/* ---------- Scene ---------- */
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
renderer.shadowMap.enabled = true;
document.body.prepend(renderer.domElement);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x87ceeb);
scene.fog = new THREE.Fog(0x87ceeb, 80, 400);
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.1, 1000);
scene.add(new THREE.HemisphereLight(0xffffff, 0x8899aa, 1.0));
const sun = new THREE.DirectionalLight(0xffffff, 1.4);
sun.castShadow = true;
sun.shadow.mapSize.set(2048, 2048);
Object.assign(sun.shadow.camera, { left: -60, right: 60, top: 60, bottom: -60, near: 1, far: 250 });
scene.add(sun, sun.target);

addEventListener('resize', () => {
  renderer.setSize(innerWidth, innerHeight);
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
});

/* ---------- Map ---------- */
const solids = [], pads = [], spinners = [], coins = [];

function addBox(x, y, z, w, h, d, color, extra = {}) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), new THREE.MeshLambertMaterial({ color }));
  m.position.set(x, y, z);
  m.castShadow = m.receiveShadow = true;
  scene.add(m);
  solids.push({ x0: x - w / 2, x1: x + w / 2, y0: y - h / 2, y1: y + h / 2, z0: z - d / 2, z1: z + d / 2, ...extra });
  return m;
}

function addPad(i, z) {
  const fin = i === STAGES;
  addBox(0, 9, z, 24, 2, 24, fin ? 0x00ff78 : new THREE.Color().setHSL((i * 0.07) % 1, 0.7, 0.6), { pad: i });
  pads[i] = new THREE.Vector3(0, 10, z);
}

function addCoin(x, y, z) {
  const geo = new THREE.CylinderGeometry(1.2, 1.2, 0.3, 24);
  geo.rotateX(Math.PI / 2);
  const m = new THREE.Mesh(geo, new THREE.MeshLambertMaterial({ color: 0xffcd00, emissive: 0x665200 }));
  m.position.set(x, y, z);
  scene.add(m);
  coins.push(m);
}

function addSpinner(x, y, z, len, speed) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(len, 1.5, 1.5), new THREE.MeshLambertMaterial({ color: 0xff2828, emissive: 0x aa0000 }));
  m.position.set(x, y, z);
  m.castShadow = true;
  scene.add(m);
  spinners.push({ mesh: m, x, z, y0: y - 0.75, y1: y + 0.75, len, speed, angle: 0 });
}

(function buildMap() {
  addPad(0, 0);
  let cursor = -12;
  for (let i = 1; i <= STAGES; i++) {
    const gap = Math.min(5 + i * 0.2, 9);
    const size = Math.max(8 - i * 0.1, 5.5);
    for (let k = 1; k <= 4; k++) {
      cursor -= gap;
      const z = cursor - size / 2;
      const top = 10 + Math.sin(k + i) * 1.5;
      addBox(0, top - 1, z, size, 2, size, new THREE.Color().setHSL((i * 0.07) % 1, 0.55, 0.6));
      if (k === 2) addCoin(0, top + 3.5, z);
      if (i % 4 === 0 && k === 2) addSpinner(0, top + 1.5, z, size * 3, 1.5 + i * 0.1);
      cursor -= size;
    }
    cursor -= gap;
    const padZ = cursor - 12;
    addPad(i, padZ);
    cursor = padZ - 12;
  }
})();

/* ---------- Avatar ---------- */
function nameSprite(text) {
  const c = document.createElement('canvas');
  c.width = 256; c.height = 64;
  const g = c.getContext('2d');
  g.fillStyle = 'rgba(0,0,0,.55)';
  g.beginPath(); g.roundRect(0, 8, 256, 48, 14); g.fill();
  g.fillStyle = '#fff'; g.font = 'bold 30px sans-serif'; g.textAlign = 'center'; g.textBaseline = 'middle';
  g.fillText(text, 128, 33);
  const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: new THREE.CanvasTexture(c), transparent: true }));
  s.scale.set(6, 1.5, 1);
  s.position.y = 6.6;
  return s;
}

function makeAvatar(color, name) {
  const g = new THREE.Group();
  const mat = c => new THREE.MeshLambertMaterial({ color: c });
  const part = (w, h, d, c, x, y, z, parent = g) => {
    const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), mat(c));
    m.position.set(x, y, z); m.castShadow = true; parent.add(m); return m;
  };
  const limb = (w, h, d, c, x, topY) => {
    const p = new THREE.Group(); p.position.set(x, topY, 0); g.add(p);
    part(w, h, d, c, 0, -h / 2, 0, p); return p;
  };
  const skin = 0xf5cd30;
  part(2, 2, 1, color, 0, 3, 0);
  part(1.2, 1.2, 1.2, skin, 0, 4.6, 0);
  part(0.2, 0.2, 0.05, 0x111111, -0.3, 4.75, 0.61);
  part(0.2, 0.2, 0.05, 0x111111, 0.3, 4.75, 0.61);
  part(0.5, 0.1, 0.05, 0x111111, 0, 4.35, 0.61);
  const limbs = {
    la: limb(1, 2, 1, skin, -1.5, 4), ra: limb(1, 2, 1, skin, 1.5, 4),
    ll: limb(1, 2, 1, 0x2d3a55, -0.5, 2), rl: limb(1, 2, 1, 0x2d3a55, 0.5, 2),
  };
  g.add(nameSprite(name));
  return { group: g, limbs, phase: 0 };
}

function animateAvatar(av, speed, dt) {
  av.phase += dt * speed * 0.7;
  const a = Math.min(speed / 16, 1) * 0.9, s = Math.sin(av.phase) * a;
  av.limbs.la.rotation.x = s; av.limbs.ra.rotation.x = -s;
  av.limbs.ll.rotation.x = -s; av.limbs.rl.rotation.x = s;
}

/* ---------- Player ---------- */
const me = { pos: new THREE.Vector3(0, 10, 0), vy: 0, grounded: false, av: null };
let stage = 0, coinCount = 0, spawn = pads[0].clone(), joined = false, yaw = 0, pitch = 0.4, dist = 22;

try {
  const s = JSON.parse(localStorage.getItem('bw') || '{}');
  stage = clamp(s.stage | 0, 0, STAGES); coinCount = s.coins | 0; spawn = pads[stage].clone();
} catch {}
const save = () => { try { localStorage.setItem('bw', JSON.stringify({ stage, coins: coinCount })); } catch {} };

function hud() { $('hud').innerHTML = `المرحلة: ${stage} / ${STAGES}<br>العملات: ${coinCount}`; }
let toastT;
function toast(t) {
  const el = $('toast'); el.textContent = t; el.style.opacity = 1;
  clearTimeout(toastT); toastT = setTimeout(() => el.style.opacity = 0, 2000);
}
function respawn() { me.pos.copy(spawn).add(new THREE.Vector3(0, 0.5, 0)); me.vy = 0; }

/* ---------- Input ---------- */
const keys = {}; let typing = false, jumpHeld = false, rotating = false;
const touch = { id: null, ox: 0, oy: 0, x: 0, y: 0 }, camTouch = { id: null, x: 0, y: 0 };

addEventListener('keydown', e => {
  if (e.code === 'Enter') {
    if (typing) { sendChat(); $('msg').blur(); } else if (joined) { $('msg').focus(); }
    e.preventDefault(); return;
  }
  if (!typing) { keys[e.code] = true; if (e.code === 'Space') e.preventDefault(); }
});
addEventListener('keyup', e => keys[e.code] = false);
$('msg').addEventListener('focus', () => { typing = true; for (const k in keys) keys[k] = false; });
$('msg').addEventListener('blur', () => typing = false);
$('jump').addEventListener('pointerdown', () => jumpHeld = true);
$('jump').addEventListener('pointerup', () => jumpHeld = false);

const cv = renderer.domElement;
cv.addEventListener('contextmenu', e => e.preventDefault());
cv.addEventListener('wheel', e => { dist = clamp(dist + e.deltaY * 0.02, 8, 50); });
cv.addEventListener('pointerdown', e => {
  if (e.pointerType === 'touch') {
    if (e.clientX < innerWidth / 2 && touch.id === null) Object.assign(touch, { id: e.pointerId, ox: e.clientX, oy: e.clientY, x: 0, y: 0 });
    else if (camTouch.id === null) Object.assign(camTouch, { id: e.pointerId, x: e.clientX, y: e.clientY });
  } else if (e.button === 2) rotating = true;
});
addEventListener('pointermove', e => {
  if (e.pointerId === touch.id) {
    touch.x = clamp((e.clientX - touch.ox) / 50, -1, 1); touch.y = clamp((e.clientY - touch.oy) / 50, -1, 1);
  } else if (e.pointerId === camTouch.id) {
    yaw -= (e.clientX - camTouch.x) * 0.006; pitch = clamp(pitch + (e.clientY - camTouch.y) * 0.006, 0.1, 1.3);
    camTouch.x = e.clientX; camTouch.y = e.clientY;
  } else if (rotating && e.pointerType === 'mouse') {
    yaw -= e.movementX * 0.005; pitch = clamp(pitch + e.movementY * 0.005, 0.1, 1.3);
  }
});
const up = e => {
  if (e.pointerId === touch.id) { touch.id = null; touch.x = touch.y = 0; }
  if (e.pointerId === camTouch.id) camTouch.id = null;
  if (e.pointerType === 'mouse' && e.button === 2) rotating = false;
  jumpHeld = false;
};
addEventListener('pointerup', up); addEventListener('pointercancel', up);

/* ---------- Network ---------- */
let ws, myId = null;
const others = new Map();

function addOther(p) {
  removeOther(p.id);
  const av = makeAvatar(p.color, p.name);
  av.group.position.set(p.x, p.y, p.z);
  scene.add(av.group);
  others.set(p.id, { av, t: new THREE.Vector3(p.x, p.y, p.z), ry: p.ry || 0, prev: new THREE.Vector3(p.x, p.y, p.z) });
}
function removeOther(id) {
  const o = others.get(id);
  if (o) { scene.remove(o.av.group); others.delete(id); }
}
function addChat(name, text) {
  const d = document.createElement('div');
  d.textContent = `${name}: ${text}`;
  $('log').appendChild(d);
  while ($('log').children.length > 30) $('log').firstChild.remove();
  $('log').scrollTop = 1e6;
}
function sendChat() {
  const text = $('msg').value.trim(); $('msg').value = '';
  if (!text) return;
  addChat(myName, text);
  if (ws && ws.readyState === 1) ws.send(JSON.stringify({ t: 'chat', text }));
}

let myName = 'Player', myColor = 0x3a86ff;
function connect() {
  try { ws = new WebSocket((location.protocol === 'https:' ? 'wss://' : 'ws://') + location.host); } catch { return; }
  ws.onopen = () => ws.send(JSON.stringify({ t: 'join', name: myName, color: myColor }));
  ws.onmessage = ev => {
    const m = JSON.parse(ev.data);
    if (m.t === 'init') { myId = m.id; m.players.forEach(addOther); }
    else if (m.t === 'join') addOther(m.p);
    else if (m.t === 'move') { const o = others.get(m.id); if (o) { o.t.set(m.x, m.y, m.z); o.ry = m.ry; } }
    else if (m.t === 'leave') removeOther(m.id);
    else if (m.t === 'chat') addChat(m.name, m.text);
  };
}

/* ---------- Game ---------- */
$('play').onclick = () => {
  myName = $('name').value.trim() || 'Player';
  myColor = parseInt($('color').value.slice(1), 16);
  me.av = makeAvatar(myColor, myName);
  scene.add(me.av.group);
  respawn(); hud(); connect();
  $('menu').style.display = 'none';
  joined = true;
};

function hit(x, y, z) {
  for (const b of solids)
    if (x + R > b.x0 && x - R < b.x1 && z + R > b.z0 && z - R < b.z1 && y + H > b.y0 && y < b.y1) return b;
  return null;
}

let sendT = 0;
function update(dt) {
  let f = (keys.KeyW || keys.ArrowUp ? 1 : 0) - (keys.KeyS || keys.ArrowDown ? 1 : 0) - touch.y;
  let r = (keys.KeyD || keys.ArrowRight ? 1 : 0) - (keys.KeyA || keys.ArrowLeft ? 1 : 0) + touch.x;
  const len = Math.hypot(f, r);
  if (len > 1) { f /= len; r /= len; }
  const dx = -Math.sin(yaw) * f + Math.cos(yaw) * r;
  const dz = -Math.cos(yaw) * f - Math.sin(yaw) * r;
  const p = me.pos;

  const nx = p.x + dx * 16 * dt; if (!hit(nx, p.y, p.z)) p.x = nx;
  const nz = p.z + dz * 16 * dt; if (!hit(p.x, p.y, nz)) p.z = nz;

  if ((keys.Space || jumpHeld) && me.grounded) { me.vy = 24; me.grounded = false; }
  me.vy = Math.max(me.vy - 60 * dt, -60);
  const ny = p.y + me.vy * dt;
  const hb = hit(p.x, ny, p.z);
  if (hb) {
    if (me.vy < 0) {
      p.y = hb.y1; me.grounded = true;
      if (hb.pad !== undefined) {
        spawn = pads[hb.pad].clone();
        if (hb.pad > stage) {
          stage = hb.pad; save(); hud();
          if (stage === STAGES) { coinCount += 50; hud(); toast('🏆 فزت باللعبة!'); } else toast('✔ نقطة حفظ ' + stage);
        }
      }
    } else p.y = hb.y0 - H - 0.01;
    me.vy = 0;
  } else { p.y = ny; me.grounded = false; }

  if (p.y < -40) respawn();

  for (const s of spinners) {
    const lx = p.x - s.x, lz = p.z - s.z, c = Math.cos(s.angle), sn = Math.sin(s.angle);
    const u = lx * c - lz * sn, w = lx * sn + lz * c;
    if (Math.abs(u) < s.len / 2 + R && Math.abs(w) < 0.75 + R && p.y < s.y1 && p.y + H > s.y0) respawn();
  }
  for (let i = coins.length - 1; i >= 0; i--)
    if (coins[i].position.distanceTo(new THREE.Vector3(p.x, p.y + 2.5, p.z)) < 3) {
      scene.remove(coins[i]); coins.splice(i, 1); coinCount++; save(); hud();
    }

  const g = me.av.group;
  g.position.copy(p);
  if (len > 0.1) {
    const t = Math.atan2(dx, dz);
    const d = ((t - g.rotation.y + Math.PI) % (2 * Math.PI) + 2 * Math.PI) % (2 * Math.PI) - Math.PI;
    g.rotation.y += d * Math.min(1, dt * 14);
  }
  animateAvatar(me.av, len * 16, dt);

  sendT += dt;
  if (sendT > 0.066 && ws && ws.readyState === 1) {
    sendT = 0;
    ws.send(JSON.stringify({ t: 'move', x: +p.x.toFixed(2), y: +p.y.toFixed(2), z: +p.z.toFixed(2), ry: +g.rotation.y.toFixed(2) }));
  }
}

let last = performance.now();
function loop(now) {
  requestAnimationFrame(loop);
  const dt = Math.min((now - last) / 1000, 0.033); last = now;

  for (const s of spinners) { s.angle += s.speed * dt; s.mesh.rotation.y = s.angle; }
  for (const c of coins) c.rotation.y += dt * 3;
  if (joined) update(dt);

  for (const o of others.values()) {
    const g = o.av.group;
    g.position.lerp(o.t, Math.min(1, dt * 12));
    g.rotation.y += (((o.ry - g.rotation.y + Math.PI) % (2 * Math.PI) + 2 * Math.PI) % (2 * Math.PI) - Math.PI) * Math.min(1, dt * 12);
    const sp = Math.hypot(g.position.x - o.prev.x, g.position.z - o.prev.z) / Math.max(dt, 1e-4);
    animateAvatar(o.av, Math.min(sp, 16), dt);
    o.prev.copy(g.position);
  }

  const t = new THREE.Vector3(me.pos.x, me.pos.y + 3, me.pos.z);
  camera.position.set(
    t.x + Math.sin(yaw) * Math.cos(pitch) * dist,
    t.y + Math.sin(pitch) * dist,
    t.z + Math.cos(yaw) * Math.cos(pitch) * dist
  );
  camera.lookAt(t);
  sun.position.set(t.x + 40, t.y + 80, t.z + 30); sun.target.position.copy(t);
  renderer.render(scene, camera);
}
requestAnimationFrame(loop);
</script>
</body>
</html>
