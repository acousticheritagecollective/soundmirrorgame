# SOUND MIRROR — Asset Replacement Manual
### Acoustic Heritage Collective, 2026

---

## File Structure

```
soundmirrorgame/
├── index.html          ← The game (single file)
├── ahc.ico             ← Favicon
├── logo.png            ← Top-left logo (always visible)
├── soundmirror.glb     ← 3D model (when ready)
├── fokker.mp3          ← Engine drone loop
├── ambient.mp3         ← Coastal ambience loop
├── gaviotas.mp3        ← Seagull calls
├── viento.mp3          ← Wind loop
├── lluvia.mp3          ← Rain loop
├── tormenta.mp3        ← Storm loop
└── truenos.mp3         ← Thunder claps (one-shot)
```

---

## 1. REPLACING AUDIO FILES

### What to prepare

| File | Duration | Loop? | Character |
|------|----------|-------|-----------|
| `fokker.mp3` | 5-8s | YES | Steady engine drone. Fundamental ~50-80Hz. NO transients. Crossfade loop points 500ms. |
| `ambient.mp3` | 15-20s | YES | Gentle coastal waves + light wind. Pink noise character. |
| `gaviotas.mp3` | 10-15s | NO | 3-4 seagull calls, irregular spacing. Triggered randomly in code. |
| `viento.mp3` | 10-15s | YES | Sustained wind, no harsh gusts. |
| `lluvia.mp3` | 10-15s | YES | Constant rain, pink noise texture. |
| `tormenta.mp3` | 15-20s | YES | Heavy rain + wind combined. NO thunder (separate file). |
| `truenos.mp3` | 8-12s | NO | 2-3 thunder claps with gaps. Triggered randomly every 8-20s. |

### How to integrate

**All audio files should be 128kbps MP3, mono or stereo.**

Open `index.html` and search for the comment:
```
// Coastal ambient: pink noise through lowpass
```

That's inside `_initAmbient()` in the `AudioEngine` class. Replace the oscillator/noise-based synthesis with MP3 loading.

#### Step-by-step for each audio:

**A) Engine drone (fokker.mp3) — in `addFormation()` method:**

Find:
```javascript
const o1=this.ctx.createOscillator(); o1.type='sawtooth';
o1.frequency.value=55+Math.random()*5;
const o2=this.ctx.createOscillator(); o2.type='square';
o2.frequency.value=27+Math.random()*3;
const mix=this.ctx.createGain(); mix.gain.value=0.4;
```

Replace the oscillators with a BufferSource. First, load the buffer once at init:

```javascript
// Add this at the end of init(), after this.ok = true:
this._loadBuffer('fokker.mp3').then(buf => { this.fokkerBuf = buf; });

// Add this method to AudioEngine:
async _loadBuffer(url) {
  const resp = await fetch(url);
  const ab = await resp.arrayBuffer();
  return this.ctx.decodeAudioData(ab);
}
```

Then in `addFormation()`, replace the oscillators:
```javascript
addFormation(id) {
  if (!this.ctx || !this.fokkerBuf) return;
  const src = this.ctx.createBufferSource();
  src.buffer = this.fokkerBuf;
  src.loop = true;
  // Slight random detune so overlapping formations sound different
  src.playbackRate.value = 0.95 + Math.random() * 0.1;
  
  const dg = this.ctx.createGain(); dg.gain.value = 0;
  const dlp = this.ctx.createBiquadFilter(); dlp.type = 'lowpass';
  dlp.frequency.value = 8000; dlp.Q.value = 0.5;
  const mg = this.ctx.createGain(); mg.gain.value = 0;
  
  src.connect(dg).connect(dlp).connect(mg).connect(this.fBus);
  src.start();
  
  this.nodes.set(id, {src, dg, dlp, mg});
}
```

And update `removeFormation()`:
```javascript
removeFormation(id) {
  const n = this.nodes.get(id);
  if (!n) return;
  const t = this.ctx.currentTime;
  n.dg.gain.setTargetAtTime(0, t, 0.1);
  setTimeout(() => {
    try { n.src.stop(); n.src.disconnect();
      n.dg.disconnect(); n.dlp.disconnect(); n.mg.disconnect();
    } catch(e) {}
    this.nodes.delete(id);
  }, 300);
}
```

**B) Ambient (ambient.mp3) — in `_initAmbient()`:**

Find:
```javascript
const nb = this._pinkBuf(3);
this.ambSrc = this.ctx.createBufferSource();
this.ambSrc.buffer = nb; this.ambSrc.loop = true;
```

Replace with:
```javascript
this._loadBuffer('ambient.mp3').then(buf => {
  this.ambSrc = this.ctx.createBufferSource();
  this.ambSrc.buffer = buf;
  this.ambSrc.loop = true;
  const lp = this.ctx.createBiquadFilter();
  lp.type = 'lowpass'; lp.frequency.value = 800; lp.Q.value = 0.5;
  this.ambSrc.connect(lp).connect(this.ambGain).connect(this.master);
  this.ambSrc.start();
});
```

**C) Weather layers (viento, lluvia, tormenta) — in `_initAmbient()`:**

Find the weather noise section:
```javascript
const wn = this.ctx.createBufferSource();
wn.buffer = this._pinkBuf(4); wn.loop = true;
```

Replace with a more sophisticated system. In `setWeather()`, load the appropriate MP3 based on the weather string:

```javascript
setWeather(boostDB, weather) {
  if (!this.ctx) return;
  const t = this.ctx.currentTime;
  const wv = boostDB > 0 ? dbGain(boostDB - 60) : 0;
  this.wxGain.gain.setTargetAtTime(wv, t, 2);
  
  // Load appropriate weather layer
  let wxFile = null;
  if (weather.includes('thunder') || weather.includes('storm')) wxFile = 'tormenta.mp3';
  else if (weather.includes('rain')) wxFile = 'lluvia.mp3';
  else if (weather.includes('wind')) wxFile = 'viento.mp3';
  
  if (wxFile && wxFile !== this._currentWxFile) {
    this._currentWxFile = wxFile;
    this._loadBuffer(wxFile).then(buf => {
      if (this._wxSrc) { try { this._wxSrc.stop(); } catch(e) {} }
      this._wxSrc = this.ctx.createBufferSource();
      this._wxSrc.buffer = buf;
      this._wxSrc.loop = true;
      this._wxSrc.connect(this.wxGain);
      this._wxSrc.start();
    });
  }
  
  // Seagulls
  if (weather.includes('seagull')) this._startSG();
  else { this.sgGain.gain.setTargetAtTime(0, t, 0.5); this._stopSG(); }
}
```

**D) Seagulls (gaviotas.mp3) — in `_startSG()`:**

Replace the oscillator-based seagull with the MP3:
```javascript
_startSG() {
  if (this._sgInt) return;
  this._loadBuffer('gaviotas.mp3').then(buf => {
    this._sgBuf = buf;
    const fire = () => {
      if (!this.ctx || !this._sgBuf) return;
      const src = this.ctx.createBufferSource();
      src.buffer = this._sgBuf;
      // Random pitch shift
      src.playbackRate.value = 0.8 + Math.random() * 0.4;
      const g = this.ctx.createGain();
      g.gain.value = 0.03;
      src.connect(g).connect(this.master);
      src.start();
      this._sgInt = setTimeout(fire, 3000 + Math.random() * 8000);
    };
    fire();
  });
}
```

**E) Thunder (truenos.mp3) — in `thunder()`:**

Replace the noise burst:
```javascript
thunder() {
  if (!this.ctx) return;
  if (!this._thunderBuf) {
    this._loadBuffer('truenos.mp3').then(buf => {
      this._thunderBuf = buf;
      this._playThunder();
    });
  } else {
    this._playThunder();
  }
}

_playThunder() {
  const src = this.ctx.createBufferSource();
  src.buffer = this._thunderBuf;
  const g = this.ctx.createGain();
  g.gain.value = 0.3;
  src.connect(g).connect(this.master);
  src.start();
}
```

---

## 2. REPLACING THE 3D MODEL (soundmirror.glb)

### What to prepare

- Export your 3D model as `.glb` (binary glTF)
- The model should face **+Z** direction (towards the sea/enemy)
- Scale: real-world meters (the mirror block is ~7m wide, ~8m tall)
- Origin: at ground level, centered on the mirror

### How to integrate

**A) Add GLTFLoader** — add this script tag after the Three.js import:

```html
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>
```

**B) Replace `_buildMirror()`** with:

```javascript
_buildMirror(T) {
  const loader = new T.GLTFLoader();
  loader.load('soundmirror.glb', (gltf) => {
    const model = gltf.scene;
    // Apply 5° forward tilt (historical)
    model.rotation.x = -5 * (Math.PI / 180);
    // Position at origin
    model.position.set(0, 0, 0);
    // Optional: scale if needed
    // model.scale.set(1, 1, 1);
    this.sc.add(model);
  }, undefined, (err) => {
    console.error('Failed to load 3D model:', err);
    // Fallback: use the placeholder geometry
    this._buildMirrorFallback(T);
  });
}
```

Rename the current `_buildMirror` to `_buildMirrorFallback` so it's used as fallback.

### Model tips

- If the model appears too dark, check that materials export correctly in glTF
- If the model is rotated wrong, adjust `model.rotation.y` (try `Math.PI` to flip 180°)
- The dish concavity should face **-Z** (towards the operator/camera)
- Test with: `model.traverse(c => { if(c.isMesh) c.material = new THREE.MeshStandardMaterial({color:0xff0000}); });` to debug visibility

---

## 3. AUDIO SPECIFICATIONS SUMMARY

### Signal chain (stays the same with MP3s)

```
[fokker.mp3 per formation, looped]
  → distanceGain (inverse square law)
  → distanceLowpass (atmospheric absorption)
  → mirrorGain (angle-dependent, ±16° table)
  → formationBus
  → stethoscope lowpass 1kHz
  → bass peaking 70Hz +12dB Q=1.2
  → horn notch 50Hz +6dB Q=3
  → master gain

[ambient.mp3 looped] → lowpass 800Hz → ambientGain → master
[weather MP3 looped] → lowpass 2kHz → weatherGain → master
[gaviotas.mp3 one-shot random] → gain → master
[truenos.mp3 one-shot random] → gain → master
```

### Critical: fokker.mp3 loop quality

This is the most important file. The player needs to hear it smoothly at varying distances/angles. Tips:

1. Record or synthesize a stable 5-8 second engine drone
2. Edit in Audacity: find a stable segment with consistent RPM
3. Apply crossfade at loop points (Edit → Preferences → Import/Export → crossfade)
4. Test the loop by playing it on repeat — any clicks or thumps will be obvious in-game
5. Export as mono MP3, 128kbps
6. Fundamental frequency should be in the 50-80Hz range (this is where the mirror gain peaks)

---

## 4. FILE PLACEMENT

Upload all files to the same directory:
```
https://acousticheritagecollective.org/soundmirrorgame/index.html
https://acousticheritagecollective.org/soundmirrorgame/ahc.ico
https://acousticheritagecollective.org/soundmirrorgame/logo.png
https://acousticheritagecollective.org/soundmirrorgame/fokker.mp3
https://acousticheritagecollective.org/soundmirrorgame/ambient.mp3
https://acousticheritagecollective.org/soundmirrorgame/gaviotas.mp3
https://acousticheritagecollective.org/soundmirrorgame/viento.mp3
https://acousticheritagecollective.org/soundmirrorgame/lluvia.mp3
https://acousticheritagecollective.org/soundmirrorgame/tormenta.mp3
https://acousticheritagecollective.org/soundmirrorgame/truenos.mp3
https://acousticheritagecollective.org/soundmirrorgame/soundmirror.glb
```

No subdirectories needed. Everything lives flat in the same folder.

---

## 5. TESTING CHECKLIST

After replacing each audio file:

- [ ] Game loads without console errors
- [ ] fokker.mp3: smooth loop, no clicks, audible volume change when sweeping horn
- [ ] ambient.mp3: smooth background, not too loud
- [ ] gaviotas.mp3: plays randomly, not jarring
- [ ] viento.mp3: activates on wind waves, smooth loop
- [ ] lluvia.mp3: activates on rain waves
- [ ] tormenta.mp3: activates on storm waves
- [ ] truenos.mp3: random thunder claps during storm waves
- [ ] Pause (ESC): complete silence
- [ ] Game over: all audio stops cleanly

After replacing the 3D model:

- [ ] Model is visible and correctly oriented
- [ ] Dish faces the operator (concave side towards -Z)
- [ ] Scale looks right relative to ground plane
- [ ] Model doesn't clip through the ground
- [ ] 5° tilt is visible

---

*Questions? The code is extensively commented. Search for `AudioEngine` and `_buildMirror` to find all relevant sections.*
