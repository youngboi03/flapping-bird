# 🐦 Jumping Bird (flapping-bird)

A Flappy Bird-style game written in **plain JavaScript and HTML5 Canvas**. The whole game ships as a **single `index.html` file** (~45 KB), with no libraries and no external audio files.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green)

## ✨ Features

- 🎮 Plug and play: open `index.html` in a browser and play
- 🎨 5 bird skins: Yellow, Red, Blue, Green, Purple
- ⚡ 3 difficulty levels: Easy, Normal, Hard
- 🌅 Day/night cycle with sun, moon and stars
- 🏙️ Two-layer parallax city background
- 🔊 8-bit sound effects and background music generated with the Web Audio API
- 📱 Responsive; works with mouse, keyboard and touch
- 🏆 Best score saved in `localStorage`

## 🚀 Running the game

```bash
git clone https://github.com/youngboi03/flapping-bird.git
cd flapping-bird
# Open index.html in a browser, or serve the folder:
python3 -m http.server 8000   # then visit http://localhost:8000
```

**Controls:** `Space` / `↑` / mouse click / touch to flap. `Esc` closes the menus.

To play online, enable **GitHub Pages** (Settings → Pages → Deploy from a branch → `main` / root).

## ⚡ Optimizations over the original (v2.2.0 → v3.0.0)

| Problem in the original | Fix |
|-------------------------|-----|
| Game speed depended on the monitor refresh rate (a 144 Hz screen ran about 2.4× too fast) | **Fixed-timestep 60 Hz** loop with an accumulator; the step delta is capped when the tab is hidden |
| Bird drawn with 5 `fillRect` calls plus strokes every frame | Each skin is **pre-rendered once** to an offscreen canvas; each frame only calls `drawImage` |
| About 20 ground stripe paths drawn every frame | Ground **texture pre-rendered** once and drawn with `drawImage` |
| `Math.sin` evaluated for every building every frame | **Building data precomputed** in a 1024-entry ring buffer |
| Theme colour strings and objects rebuilt every frame | Colours are **recalculated only when the day/night factor changes** |
| A new hitbox object per pipe per frame | Collision is **inline with no allocations** and exits early on hit |
| `Array.splice` and `ctx.save/restore` for particles | **Swap-remove**, a cap of 60 particles, no save/restore |
| `clearRect` before a full-screen background draw | Removed; canvas uses `alpha: false` |
| Separate `mousedown` and `touchstart` handlers | A single **`pointerdown`** handler |
| `AudioContext` created on page load; playlists rebuilt on every track change | **AudioContext created lazily** after a user gesture; playlists built once; audio nodes disconnected when done |
| `@import` for the font blocked rendering | `<link rel="preconnect">` plus `display=swap` |

### 🐛 Bug fixes

- Clicking inside the SKINS or LEVEL menu started the game unintentionally
- Holding Space made the bird flap continuously (key repeat)
- Blocked `localStorage` (e.g. private mode) could stop the game from running
- Resizing the window reset the canvas
- Music kept playing after switching to another tab
- The default difficulty button did not show the selected state

## ⚙️ Customization

Open `index.html` and find the `CONFIG` object:

```javascript
const CONFIG = {
    gravity: 0.25,       // gravity
    jumpStrength: -4.5,  // flap strength (negative = upward)
    pipeSpeed: 2,        // pipe speed
    difficulties: { /* gap and spawnRate per level */ },
    characters: [ /* add a skin: { name, body, outline, wing } */ ]
};
```

## 📁 Project structure

```
flapping-bird/
├── index.html   # the whole game (CSS + HTML + JS)
├── README.md
├── LICENSE      # MIT
└── .gitignore
```

The JavaScript in `index.html` is organised as: `CONFIG` → `Utils` → `AudioSys` → `BirdSprite` → `Bird` → `Pipes` → `Particles` → `Environment` → `Game`.

## 📜 License

[MIT](LICENSE)
