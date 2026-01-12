# San Andreas Drum Machine Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a web-based drum machine sequencer with auto-generated G-Funk/West Coast beats and San Andreas-inspired synthesized sounds.

**Architecture:** Single HTML file with embedded CSS and JavaScript using the Web Audio API. 16-step x 8-track grid sequencer with pattern randomization based on genre-weighted probabilities. All sounds synthesized in real-time (no samples).

**Tech Stack:** HTML5, CSS3, JavaScript (ES6+), Web Audio API

---

## Task 1: Project Scaffold & Basic HTML Structure

**Files:**
- Create: `index.html`

**Step 1: Create the HTML skeleton**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LUCKY CHARMS - San Andreas Drum Machine</title>
    <style>
        /* Styles will go here */
    </style>
</head>
<body>
    <div id="app">
        <header>
            <h1>LUCKY CHARMS</h1>
            <p class="subtitle">San Andreas Beat Machine</p>
        </header>
        <main>
            <section id="controls"></section>
            <section id="sequencer"></section>
            <section id="mixer"></section>
        </main>
    </div>
    <script>
        // JavaScript will go here
    </script>
</body>
</html>
```

**Step 2: Open in browser to verify it loads**

Open `index.html` in browser. Expected: See "LUCKY CHARMS" heading.

---

## Task 2: Retro GTA-Inspired CSS Styling

**Files:**
- Modify: `index.html` (style section)

**Step 1: Add the complete CSS**

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #1a0a2e 0%, #16213e 50%, #0f3460 100%);
    min-height: 100vh;
    color: #fff;
    overflow-x: hidden;
}

#app {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

header {
    text-align: center;
    margin-bottom: 30px;
    padding: 20px;
    background: rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    border: 2px solid #ff6b35;
    box-shadow: 0 0 20px rgba(255, 107, 53, 0.3);
}

h1 {
    font-size: 3rem;
    text-transform: uppercase;
    letter-spacing: 8px;
    background: linear-gradient(90deg, #ff6b35, #f7c531, #ff6b35);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-shadow: none;
    animation: glow 2s ease-in-out infinite alternate;
}

@keyframes glow {
    from { filter: drop-shadow(0 0 5px #ff6b35); }
    to { filter: drop-shadow(0 0 20px #f7c531); }
}

.subtitle {
    color: #00ff88;
    font-size: 1.2rem;
    letter-spacing: 4px;
    margin-top: 10px;
    text-transform: uppercase;
}

#controls {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 30px;
    flex-wrap: wrap;
}

.btn {
    padding: 15px 30px;
    font-size: 1rem;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 2px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.btn-play {
    background: linear-gradient(180deg, #00ff88, #00cc6a);
    color: #000;
}

.btn-play:hover {
    transform: scale(1.05);
    box-shadow: 0 0 30px rgba(0, 255, 136, 0.5);
}

.btn-play.playing {
    background: linear-gradient(180deg, #ff4444, #cc0000);
}

.btn-generate {
    background: linear-gradient(180deg, #f7c531, #ff6b35);
    color: #000;
}

.btn-generate:hover {
    transform: scale(1.05);
    box-shadow: 0 0 30px rgba(247, 197, 49, 0.5);
}

.btn-clear {
    background: linear-gradient(180deg, #666, #444);
    color: #fff;
}

.btn-clear:hover {
    transform: scale(1.05);
}

.control-group {
    display: flex;
    align-items: center;
    gap: 10px;
    background: rgba(0, 0, 0, 0.3);
    padding: 10px 20px;
    border-radius: 5px;
}

.control-group label {
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.control-group input[type="range"] {
    width: 100px;
    accent-color: #ff6b35;
}

.control-group span {
    min-width: 50px;
    text-align: center;
    color: #00ff88;
    font-weight: bold;
}

#sequencer {
    background: rgba(0, 0, 0, 0.4);
    border-radius: 10px;
    padding: 20px;
    border: 2px solid #333;
    margin-bottom: 20px;
}

.track {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
    gap: 10px;
}

.track-label {
    width: 100px;
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #aaa;
    text-align: right;
    padding-right: 10px;
}

.steps {
    display: flex;
    gap: 4px;
}

.step {
    width: 40px;
    height: 40px;
    background: rgba(255, 255, 255, 0.1);
    border: 2px solid #333;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.15s ease;
}

.step:hover {
    background: rgba(255, 255, 255, 0.2);
    border-color: #555;
}

.step.active {
    background: linear-gradient(180deg, #ff6b35, #cc4400);
    border-color: #ff6b35;
    box-shadow: 0 0 10px rgba(255, 107, 53, 0.5);
}

.step.current {
    border-color: #00ff88;
    box-shadow: 0 0 15px rgba(0, 255, 136, 0.7);
}

.step.beat-marker {
    border-bottom: 3px solid #f7c531;
}

#mixer {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 15px;
    background: rgba(0, 0, 0, 0.3);
    padding: 20px;
    border-radius: 10px;
}

.mixer-channel {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    padding: 15px;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 8px;
}

.mixer-channel label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #888;
}

.mixer-channel input[type="range"] {
    writing-mode: vertical-lr;
    direction: rtl;
    height: 80px;
    accent-color: #00ff88;
}

.mixer-channel .volume-value {
    color: #00ff88;
    font-size: 0.85rem;
    font-weight: bold;
}
```

**Step 2: Refresh browser to verify styling**

Expected: Dark gradient background, neon orange/green accents, retro GTA vibe.

---

## Task 3: Controls HTML Structure

**Files:**
- Modify: `index.html` (controls section)

**Step 1: Add control buttons and BPM/Swing sliders**

Replace the controls section:

```html
<section id="controls">
    <button class="btn btn-play" id="playBtn">Play</button>
    <button class="btn btn-generate" id="generateBtn">Generate Beat</button>
    <button class="btn btn-clear" id="clearBtn">Clear</button>
    <div class="control-group">
        <label for="bpm">BPM</label>
        <input type="range" id="bpm" min="60" max="180" value="95">
        <span id="bpmValue">95</span>
    </div>
    <div class="control-group">
        <label for="swing">Swing</label>
        <input type="range" id="swing" min="0" max="100" value="20">
        <span id="swingValue">20%</span>
    </div>
</section>
```

**Step 2: Verify controls render**

Expected: Play/Generate/Clear buttons visible with BPM and Swing sliders.

---

## Task 4: Sequencer Grid HTML Generation

**Files:**
- Modify: `index.html` (script section)

**Step 1: Add track configuration and grid generation**

```javascript
// Track Configuration
const TRACKS = [
    { id: 'kick', name: 'Kick', color: '#ff6b35' },
    { id: 'snare', name: 'Snare', color: '#f7c531' },
    { id: 'clap', name: 'Clap', color: '#ff4488' },
    { id: 'hihatClosed', name: 'HH Closed', color: '#00ff88' },
    { id: 'hihatOpen', name: 'HH Open', color: '#00cc6a' },
    { id: 'bass', name: 'Synth Bass', color: '#4488ff' },
    { id: 'lead', name: 'Synth Lead', color: '#aa44ff' },
    { id: 'fx', name: 'FX', color: '#ff44aa' }
];

const STEPS = 16;

// Pattern State
const pattern = {};
TRACKS.forEach(track => {
    pattern[track.id] = new Array(STEPS).fill(false);
});

// Generate Sequencer Grid
function createSequencerGrid() {
    const sequencer = document.getElementById('sequencer');
    sequencer.innerHTML = '';

    TRACKS.forEach(track => {
        const trackDiv = document.createElement('div');
        trackDiv.className = 'track';

        const label = document.createElement('div');
        label.className = 'track-label';
        label.textContent = track.name;
        label.style.color = track.color;
        trackDiv.appendChild(label);

        const stepsDiv = document.createElement('div');
        stepsDiv.className = 'steps';

        for (let i = 0; i < STEPS; i++) {
            const step = document.createElement('div');
            step.className = 'step';
            step.dataset.track = track.id;
            step.dataset.step = i;

            // Beat markers (every 4 steps)
            if (i % 4 === 0) {
                step.classList.add('beat-marker');
            }

            step.addEventListener('click', () => toggleStep(track.id, i));
            stepsDiv.appendChild(step);
        }

        trackDiv.appendChild(stepsDiv);
        sequencer.appendChild(trackDiv);
    });
}

function toggleStep(trackId, stepIndex) {
    pattern[trackId][stepIndex] = !pattern[trackId][stepIndex];
    updateGridDisplay();
}

function updateGridDisplay() {
    TRACKS.forEach(track => {
        for (let i = 0; i < STEPS; i++) {
            const step = document.querySelector(`.step[data-track="${track.id}"][data-step="${i}"]`);
            if (step) {
                step.classList.toggle('active', pattern[track.id][i]);
            }
        }
    });
}

// Initialize on load
document.addEventListener('DOMContentLoaded', () => {
    createSequencerGrid();
});
```

**Step 2: Verify grid renders and clicking toggles steps**

Expected: 8 tracks x 16 steps grid. Clicking a step turns it orange.

---

## Task 5: Mixer HTML Generation

**Files:**
- Modify: `index.html` (script section - add to existing)

**Step 1: Add mixer state and generation**

```javascript
// Volume State
const volumes = {};
TRACKS.forEach(track => {
    volumes[track.id] = 0.7;
});

// Generate Mixer
function createMixer() {
    const mixer = document.getElementById('mixer');
    mixer.innerHTML = '';

    TRACKS.forEach(track => {
        const channel = document.createElement('div');
        channel.className = 'mixer-channel';

        const label = document.createElement('label');
        label.textContent = track.name;
        label.style.color = track.color;
        channel.appendChild(label);

        const slider = document.createElement('input');
        slider.type = 'range';
        slider.min = '0';
        slider.max = '100';
        slider.value = '70';
        slider.dataset.track = track.id;
        slider.addEventListener('input', (e) => {
            volumes[track.id] = e.target.value / 100;
            e.target.nextElementSibling.textContent = e.target.value + '%';
        });
        channel.appendChild(slider);

        const value = document.createElement('span');
        value.className = 'volume-value';
        value.textContent = '70%';
        channel.appendChild(value);

        mixer.appendChild(channel);
    });
}

// Update DOMContentLoaded
document.addEventListener('DOMContentLoaded', () => {
    createSequencerGrid();
    createMixer();
});
```

**Step 2: Verify mixer renders**

Expected: 8 vertical sliders with track names.

---

## Task 6: Web Audio API Setup & Drum Synthesis

**Files:**
- Modify: `index.html` (script section - add before track config)

**Step 1: Add AudioContext and drum synthesis functions**

```javascript
// Audio Context
let audioCtx = null;

function initAudio() {
    if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    if (audioCtx.state === 'suspended') {
        audioCtx.resume();
    }
}

// 808-style Kick Drum
function playKick(time, volume) {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();

    osc.type = 'sine';
    osc.frequency.setValueAtTime(150, time);
    osc.frequency.exponentialRampToValueAtTime(30, time + 0.15);

    gain.gain.setValueAtTime(volume, time);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.4);

    osc.connect(gain);
    gain.connect(audioCtx.destination);

    osc.start(time);
    osc.stop(time + 0.4);
}

// Snare Drum (noise + triangle)
function playSnare(time, volume) {
    // Noise component
    const bufferSize = audioCtx.sampleRate * 0.2;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);
    for (let i = 0; i < bufferSize; i++) {
        data[i] = Math.random() * 2 - 1;
    }

    const noise = audioCtx.createBufferSource();
    noise.buffer = buffer;

    const noiseFilter = audioCtx.createBiquadFilter();
    noiseFilter.type = 'highpass';
    noiseFilter.frequency.value = 1000;

    const noiseGain = audioCtx.createGain();
    noiseGain.gain.setValueAtTime(volume * 0.8, time);
    noiseGain.gain.exponentialRampToValueAtTime(0.001, time + 0.2);

    noise.connect(noiseFilter);
    noiseFilter.connect(noiseGain);
    noiseGain.connect(audioCtx.destination);

    // Triangle body
    const osc = audioCtx.createOscillator();
    osc.type = 'triangle';
    osc.frequency.value = 180;

    const oscGain = audioCtx.createGain();
    oscGain.gain.setValueAtTime(volume * 0.5, time);
    oscGain.gain.exponentialRampToValueAtTime(0.001, time + 0.1);

    osc.connect(oscGain);
    oscGain.connect(audioCtx.destination);

    noise.start(time);
    noise.stop(time + 0.2);
    osc.start(time);
    osc.stop(time + 0.1);
}

// Clap (layered noise bursts)
function playClap(time, volume) {
    for (let i = 0; i < 3; i++) {
        const bufferSize = audioCtx.sampleRate * 0.02;
        const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
        const data = buffer.getChannelData(0);
        for (let j = 0; j < bufferSize; j++) {
            data[j] = Math.random() * 2 - 1;
        }

        const noise = audioCtx.createBufferSource();
        noise.buffer = buffer;

        const filter = audioCtx.createBiquadFilter();
        filter.type = 'bandpass';
        filter.frequency.value = 2000;
        filter.Q.value = 1;

        const gain = audioCtx.createGain();
        const offset = i * 0.01;
        gain.gain.setValueAtTime(volume * 0.5, time + offset);
        gain.gain.exponentialRampToValueAtTime(0.001, time + offset + 0.1);

        noise.connect(filter);
        filter.connect(gain);
        gain.connect(audioCtx.destination);

        noise.start(time + offset);
        noise.stop(time + offset + 0.1);
    }
}

// Closed Hi-Hat
function playHihatClosed(time, volume) {
    const bufferSize = audioCtx.sampleRate * 0.05;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);
    for (let i = 0; i < bufferSize; i++) {
        data[i] = Math.random() * 2 - 1;
    }

    const noise = audioCtx.createBufferSource();
    noise.buffer = buffer;

    const filter = audioCtx.createBiquadFilter();
    filter.type = 'highpass';
    filter.frequency.value = 7000;

    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(volume * 0.3, time);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.05);

    noise.connect(filter);
    filter.connect(gain);
    gain.connect(audioCtx.destination);

    noise.start(time);
    noise.stop(time + 0.05);
}

// Open Hi-Hat
function playHihatOpen(time, volume) {
    const bufferSize = audioCtx.sampleRate * 0.3;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);
    for (let i = 0; i < bufferSize; i++) {
        data[i] = Math.random() * 2 - 1;
    }

    const noise = audioCtx.createBufferSource();
    noise.buffer = buffer;

    const filter = audioCtx.createBiquadFilter();
    filter.type = 'highpass';
    filter.frequency.value = 6000;

    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(volume * 0.25, time);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.3);

    noise.connect(filter);
    filter.connect(gain);
    gain.connect(audioCtx.destination);

    noise.start(time);
    noise.stop(time + 0.3);
}
```

**Step 2: Test in browser console**

Open dev tools, run: `initAudio(); playKick(audioCtx.currentTime, 0.7);`
Expected: Hear a deep 808 kick.

---

## Task 7: Synth Bass & Lead Synthesis

**Files:**
- Modify: `index.html` (script section - add after drum functions)

**Step 1: Add Moog-style bass and detuned lead synths**

```javascript
// G-Funk Synth Bass (Moog-style)
function playBass(time, volume) {
    const osc1 = audioCtx.createOscillator();
    const osc2 = audioCtx.createOscillator();
    const filter = audioCtx.createBiquadFilter();
    const gain = audioCtx.createGain();

    // Two detuned saw waves
    osc1.type = 'sawtooth';
    osc2.type = 'sawtooth';

    // G-Funk uses notes around E1-G2, pick random from scale
    const bassNotes = [41.2, 46.25, 49, 55, 61.74, 65.41]; // E1, G#1, A1, etc
    const freq = bassNotes[Math.floor(Math.random() * bassNotes.length)];

    osc1.frequency.value = freq;
    osc2.frequency.value = freq * 1.005; // Slight detune

    // Low-pass filter with envelope (Moog style)
    filter.type = 'lowpass';
    filter.frequency.setValueAtTime(2000, time);
    filter.frequency.exponentialRampToValueAtTime(200, time + 0.3);
    filter.Q.value = 5;

    gain.gain.setValueAtTime(volume * 0.4, time);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.4);

    osc1.connect(filter);
    osc2.connect(filter);
    filter.connect(gain);
    gain.connect(audioCtx.destination);

    osc1.start(time);
    osc2.start(time);
    osc1.stop(time + 0.4);
    osc2.stop(time + 0.4);
}

// G-Funk Synth Lead (talk-box style)
function playLead(time, volume) {
    const osc1 = audioCtx.createOscillator();
    const osc2 = audioCtx.createOscillator();
    const filter = audioCtx.createBiquadFilter();
    const gain = audioCtx.createGain();

    // Lead notes (higher register)
    const leadNotes = [329.63, 392, 440, 493.88, 523.25, 587.33]; // E4-D5
    const freq = leadNotes[Math.floor(Math.random() * leadNotes.length)];

    osc1.type = 'sawtooth';
    osc2.type = 'square';
    osc1.frequency.value = freq;
    osc2.frequency.value = freq * 2.01; // Octave up + detune

    // Wah-like filter sweep
    filter.type = 'bandpass';
    filter.frequency.setValueAtTime(500, time);
    filter.frequency.exponentialRampToValueAtTime(3000, time + 0.1);
    filter.frequency.exponentialRampToValueAtTime(800, time + 0.3);
    filter.Q.value = 3;

    gain.gain.setValueAtTime(0, time);
    gain.gain.linearRampToValueAtTime(volume * 0.2, time + 0.02);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.35);

    osc1.connect(filter);
    osc2.connect(filter);
    filter.connect(gain);
    gain.connect(audioCtx.destination);

    osc1.start(time);
    osc2.start(time);
    osc1.stop(time + 0.35);
    osc2.stop(time + 0.35);
}

// FX - Vinyl scratch / noise hit
function playFx(time, volume) {
    const bufferSize = audioCtx.sampleRate * 0.15;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);

    for (let i = 0; i < bufferSize; i++) {
        // Descending filtered noise (scratch effect)
        data[i] = (Math.random() * 2 - 1) * (1 - i / bufferSize);
    }

    const noise = audioCtx.createBufferSource();
    noise.buffer = buffer;

    const filter = audioCtx.createBiquadFilter();
    filter.type = 'bandpass';
    filter.frequency.setValueAtTime(4000, time);
    filter.frequency.exponentialRampToValueAtTime(500, time + 0.15);
    filter.Q.value = 2;

    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(volume * 0.3, time);
    gain.gain.exponentialRampToValueAtTime(0.001, time + 0.15);

    noise.connect(filter);
    filter.connect(gain);
    gain.connect(audioCtx.destination);

    noise.start(time);
    noise.stop(time + 0.15);
}

// Sound dispatch function
function playSound(trackId, time, volume) {
    switch(trackId) {
        case 'kick': playKick(time, volume); break;
        case 'snare': playSnare(time, volume); break;
        case 'clap': playClap(time, volume); break;
        case 'hihatClosed': playHihatClosed(time, volume); break;
        case 'hihatOpen': playHihatOpen(time, volume); break;
        case 'bass': playBass(time, volume); break;
        case 'lead': playLead(time, volume); break;
        case 'fx': playFx(time, volume); break;
    }
}
```

**Step 2: Test synths in console**

Run: `initAudio(); playBass(audioCtx.currentTime, 0.7); playLead(audioCtx.currentTime + 0.5, 0.5);`
Expected: Hear Moog-style bass and wah-like lead.

---

## Task 8: Sequencer Playback Engine

**Files:**
- Modify: `index.html` (script section)

**Step 1: Add sequencer state and playback loop**

```javascript
// Playback State
let isPlaying = false;
let currentStep = 0;
let bpm = 95;
let swing = 20;
let nextNoteTime = 0;
let schedulerTimerId = null;

const SCHEDULE_AHEAD_TIME = 0.1; // seconds
const SCHEDULER_INTERVAL = 25; // milliseconds

function getBpmValue() {
    return parseInt(document.getElementById('bpm').value);
}

function getSwingValue() {
    return parseInt(document.getElementById('swing').value);
}

function getStepDuration() {
    // 16th note duration
    return 60 / getBpmValue() / 4;
}

function getSwingOffset(step) {
    // Apply swing to off-beat 16ths (odd steps)
    if (step % 2 === 1) {
        return getStepDuration() * (getSwingValue() / 100) * 0.5;
    }
    return 0;
}

function scheduler() {
    while (nextNoteTime < audioCtx.currentTime + SCHEDULE_AHEAD_TIME) {
        scheduleStep(currentStep, nextNoteTime);
        advanceStep();
    }
}

function scheduleStep(step, time) {
    // Update visual indicator
    setTimeout(() => {
        document.querySelectorAll('.step.current').forEach(el => el.classList.remove('current'));
        document.querySelectorAll(`.step[data-step="${step}"]`).forEach(el => el.classList.add('current'));
    }, (time - audioCtx.currentTime) * 1000);

    // Play sounds for active steps
    TRACKS.forEach(track => {
        if (pattern[track.id][step]) {
            playSound(track.id, time, volumes[track.id]);
        }
    });
}

function advanceStep() {
    const stepDuration = getStepDuration();
    const swingOffset = getSwingOffset(currentStep);
    nextNoteTime += stepDuration + swingOffset;
    currentStep = (currentStep + 1) % STEPS;
}

function startPlayback() {
    initAudio();
    isPlaying = true;
    currentStep = 0;
    nextNoteTime = audioCtx.currentTime;
    schedulerTimerId = setInterval(scheduler, SCHEDULER_INTERVAL);
    document.getElementById('playBtn').textContent = 'Stop';
    document.getElementById('playBtn').classList.add('playing');
}

function stopPlayback() {
    isPlaying = false;
    clearInterval(schedulerTimerId);
    document.querySelectorAll('.step.current').forEach(el => el.classList.remove('current'));
    document.getElementById('playBtn').textContent = 'Play';
    document.getElementById('playBtn').classList.remove('playing');
}

function togglePlayback() {
    if (isPlaying) {
        stopPlayback();
    } else {
        startPlayback();
    }
}
```

**Step 2: Wire up play button**

Add to DOMContentLoaded:

```javascript
document.getElementById('playBtn').addEventListener('click', togglePlayback);
```

**Step 3: Test playback**

Click Play with some steps active. Expected: Sequencer loops, current step highlighted green.

---

## Task 9: BPM and Swing Controls

**Files:**
- Modify: `index.html` (script section - add to DOMContentLoaded)

**Step 1: Wire up BPM and Swing sliders**

```javascript
// BPM control
document.getElementById('bpm').addEventListener('input', (e) => {
    document.getElementById('bpmValue').textContent = e.target.value;
});

// Swing control
document.getElementById('swing').addEventListener('input', (e) => {
    document.getElementById('swingValue').textContent = e.target.value + '%';
});
```

**Step 2: Test controls while playing**

Change BPM slider while playing. Expected: Tempo changes in real-time.

---

## Task 10: Beat Generation Algorithm

**Files:**
- Modify: `index.html` (script section)

**Step 1: Add G-Funk weighted pattern generator**

```javascript
// G-Funk Pattern Generation Weights
const GFUNK_PATTERNS = {
    kick: {
        // Kicks on 1, 5, 9, 13 with variations
        weights: [0.95, 0.1, 0.05, 0.2, 0.9, 0.05, 0.15, 0.3, 0.85, 0.1, 0.05, 0.2, 0.9, 0.05, 0.15, 0.1]
    },
    snare: {
        // Snares on 5 and 13 (backbeat)
        weights: [0, 0, 0, 0, 0.95, 0, 0, 0, 0, 0, 0, 0, 0.95, 0, 0, 0]
    },
    clap: {
        // Claps layered with snare, occasional extra
        weights: [0, 0, 0, 0, 0.8, 0, 0, 0.15, 0, 0, 0, 0, 0.8, 0, 0.1, 0]
    },
    hihatClosed: {
        // Steady 8ths or 16ths
        weights: [0.9, 0.6, 0.9, 0.6, 0.9, 0.6, 0.9, 0.6, 0.9, 0.6, 0.9, 0.6, 0.9, 0.6, 0.9, 0.6]
    },
    hihatOpen: {
        // Occasional open hats
        weights: [0, 0, 0.3, 0, 0, 0, 0.25, 0, 0, 0, 0.3, 0, 0, 0, 0.25, 0]
    },
    bass: {
        // Bass follows kick loosely
        weights: [0.9, 0, 0.2, 0.15, 0.1, 0.1, 0.3, 0.2, 0.85, 0, 0.2, 0.15, 0.1, 0.1, 0.3, 0]
    },
    lead: {
        // Sparse melodic hits
        weights: [0.3, 0, 0, 0.2, 0, 0, 0.35, 0, 0.25, 0, 0, 0.3, 0, 0, 0.2, 0]
    },
    fx: {
        // Very sparse FX
        weights: [0.1, 0, 0, 0, 0, 0, 0, 0.15, 0, 0, 0, 0, 0, 0, 0, 0.1]
    }
};

function generateBeat() {
    TRACKS.forEach(track => {
        const weights = GFUNK_PATTERNS[track.id].weights;
        for (let i = 0; i < STEPS; i++) {
            pattern[track.id][i] = Math.random() < weights[i];
        }
    });
    updateGridDisplay();
}

function clearPattern() {
    TRACKS.forEach(track => {
        pattern[track.id].fill(false);
    });
    updateGridDisplay();
}
```

**Step 2: Wire up Generate and Clear buttons**

Add to DOMContentLoaded:

```javascript
document.getElementById('generateBtn').addEventListener('click', generateBeat);
document.getElementById('clearBtn').addEventListener('click', clearPattern);
```

**Step 3: Test generation**

Click "Generate Beat" multiple times. Expected: Different G-Funk style patterns each time.

---

## Task 11: Final Polish & Complete File Assembly

**Files:**
- Modify: `index.html` (assemble everything)

**Step 1: Combine all code into final index.html**

Create the complete, assembled file with all CSS and JS integrated.

**Step 2: Final test checklist**

- [ ] Open index.html in browser
- [ ] Click "Generate Beat" - pattern appears
- [ ] Click "Play" - hear G-Funk beat
- [ ] Adjust BPM slider - tempo changes
- [ ] Adjust Swing slider - groove changes
- [ ] Click individual steps - toggle on/off
- [ ] Adjust mixer volumes - individual track levels change
- [ ] Click "Clear" - pattern clears
- [ ] Click "Stop" - playback stops

---

## Summary

**Total Tasks:** 11
**Estimated Steps:** ~35 individual actions
**Output:** Single `index.html` file (~500 lines)

**Key Features:**
- 16-step x 8-track sequencer grid
- All sounds synthesized via Web Audio API (no samples)
- G-Funk weighted pattern generation
- BPM and Swing controls
- Per-track volume mixer
- Retro GTA San Andreas visual aesthetic
