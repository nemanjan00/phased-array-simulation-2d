# 2D Phased Array Simulation

An interactive, single-file playground for wave interference and phased-array
beamforming. Drag antennas around, phase them by hand or by script, steer the
beam, and watch the far-field radiation pattern react in real time.

**No build, no dependencies** — everything lives in one `index.html`. Open it
in a browser and you're running.

## Features

### Simulation
- Real-time 2D interference field from up to **16 antennas**, rendered on
  canvas with a precomputed color LUT and per-antenna cached distance fields
- Color encodes the **instantaneous field amplitude** (blue −1 → green 0 →
  red +1, see the on-screen legend)
- Togglable **distance fading** (cylindrical-wave ~1/√r decay), shown honestly
  in the field value itself
- **Zoom** with the mouse wheel, pinch gesture, or slider — a pure view
  transform that never changes the physics
- Adjustable wavelength, animation speed, render resolution, play/pause

### Antennas
- **Drag** the glowing markers to move antennas; add/remove them freely
- Per-antenna **phase** (0–2π) and **TX power** (0–2) sliders — great for
  demonstrating power imbalance (watch the pattern nulls fill in)
- **Presets** for classic beamforming setups: broadside / endfire / steered
  uniform linear arrays, a circular array, a cardioid pair, a grating-lobe
  demo, and an imbalanced pair

### Radiation pattern
- Live **polar plot of the normalized array factor** |AF(θ)|, recomputed every
  frame from the actual antenna positions, phases, and powers
- Same wavenumber and screen-coordinate convention as the simulation, so the
  lobes point exactly where the fringes constructively interfere
- Hover for an angle/|AF| readout, **click to steer the beam** to that angle
- **Swap** button to exchange the sim and the pattern (either can be the main
  view)

### Beam steering
- Pick a direction (slider with a live direction arrow, or click the pattern
  chart) and the tool applies **conjugate phasing** — φₖ = k·(rₖ·û) — so the
  main lobe points exactly there

### Scripting
- In-page code editor with syntax highlighting; your code runs every frame as
  `update(t, antennas, dt)`
- Mutate `antennas[i].x / .y / .phase / .amp` to animate the array:

  ```js
  // orbit antenna 2 while sweeping its phase
  const a = antennas[1];
  if (a) {
      a.x = 0.5 + 0.25 * Math.cos(t / 2);
      a.y = 0.5 + 0.25 * Math.sin(t / 2);
      a.phase = t;
  }
  ```

## Usage

```sh
git clone git@github.com:nemanjan00/phased-array-simulation-2d.git
cd phased-array-simulation-2d
# then open index.html in a browser — that's it
```

Things to try:

1. Load the **ULA 8 · λ/2 · broadside** preset and drag the steering slider —
   watch the lobe swing and grating lobes appear near endfire
2. Load the **Imbalanced pair** preset and compare its filled-in nulls with a
   balanced pair's perfect zeros
3. Turn on fading and zoom out to see the field decay toward the green
   zero-line of the legend
4. Script a moving array and watch the radiation pattern track it live

## Conventions

- Angles are **screen coordinates**: 0° points right, 90° points down,
  increasing clockwise — the pattern chart and the canvas always agree
- The wavelength slider is the true fringe spacing in world pixels
  (k = 2π/λ everywhere: simulation, array factor, and steering)
- Antenna positions are normalized to the stage, so layouts survive window
  resizes; zoom is view-only

## License

Do whatever you like with it. If it helps you teach or learn beamforming,
that's the point.
