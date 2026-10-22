# Pole Racer

![Version](https://img.shields.io/badge/version-1.14-blue)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?logo=javascript&logoColor=black)
![Canvas 2D](https://img.shields.io/badge/rendering-Canvas_2D-informational)
![Single file](https://img.shields.io/badge/distribution-single_file-lightgrey)
![Input](https://img.shields.io/badge/input-keyboard_%7C_touch_%7C_gamepad-orange)

An arcade racing game in the spirit of the 1982 classic *Pole Position*, written as a single self-contained `index.html`: pseudo-3D road rendering on a 2D canvas, synthesized sound, no assets, no build step, no dependencies.

> Pole Racer is an original work inspired by the genre of early-80s arcade racers. It is not affiliated with, endorsed by, or derived from the code or assets of any commercial title.

## Quick start

Open `index.html` in a modern browser. That is all.

To serve it locally instead (for example to test on a phone on the same network):

```sh
python3 -m http.server 8000
```

then open `http://<your-ip>:8000/` on the device.

## Game modes

From the title screen, use **Up/Down** to select a row and **Left/Right** to change its value.

| Row | Description |
| --- | --- |
| **Arcade** | Qualifying lap, then a 3-lap race against 7 cars. Beat the clock: each lap adds time. After the race the next circuit starts; after the ninth the cycle repeats with a harder field. |
| **Training** | Free driving on a circuit of your choice, with no traffic, no clock and no score. Laps are counted and timed. Pause and choose *Exit to menu* to leave. |
| **Mode** | Driving difficulty: **Easy**, **Intermediate**, **Expert** (see below). The choice is saved in the browser. |
| **Time** | **Day** or **Night**. Applies to every circuit, in Arcade and Training. The choice is saved in the browser. |

### Driving modes

The modes differ in how much the road pulls the car sideways in a bend, and therefore in how much you have to slow down.

| Mode | Behaviour |
| --- | --- |
| **Easy** | Every corner of every circuit can be taken at full speed. The road pulls the car sideways immediately. |
| **Intermediate** | Wide corners can still be taken flat out; the tighter the corner, the more speed you must shed or you run wide. The tightest corners need roughly two thirds of top speed. |
| **Expert** | Even the widest corners require a small lift, and tight ones need substantial braking. Time limits are more generous to compensate. |

In Intermediate and Expert the car does not snap off the road at the start of a bend: the sideways push builds up gradually, as if the car had lateral inertia, and fades gradually after the bend. This leaves time to react by steering or lifting, but taking a tight bend flat out still ends off the road.

### Minimap

A small schematic map of the circuit is shown at the right edge under the lap time, with the finish line, your car (yellow) and, in a race, the other cars (red). Tunnels are drawn in orange and bridges in blue. Toggle it with `T`; the choice is saved in the browser. The outline is derived from the track's curvature and forced to close, so it is schematic and not to scale.

### Night

In night mode the scenery is dark and the road is lit only by the car's headlights: a cone of light ahead of the car, fixed to the car's heading. On bends the road leaves the beam, so you see less of what is coming and must brake earlier. Other cars appear as dark shapes with red tail lights. Night mode can be combined with any driving mode. In Training it can also be toggled at any time with `N`.

### Tunnel

The tunnel on Riviera Streets is dark in both day and night: the road and walls are visible only within the reach of the headlights, and the exit shows as a distant patch of daylight.

Leaving the road slows the car heavily; hitting scenery, tunnel walls, rock walls, bridge edges or going over a precipice edge causes a crash and a short respawn.

## Circuits

1. **Mount Circuit**: mountain backdrop, trees.
2. **Harbor Run**: seaside, palms, city skyline.
3. **Dusk Desert**: sunset mesas and cacti.
4. **Riviera Streets**: coastal city circuit with a long, dark tunnel section.
5. **Cascade Valley**: alpine valley with a river that runs along the whole circuit and crosses under four bridges, with wooded slopes on both sides and waterfalls pouring down them. Water and falls are animated.
6. **Pine Forest**: dense conifer woodland with trees close to the track.
7. **Coast Road**: road cut into a mountainside: a jagged rock wall on the left and a precipice over the sea on the right, with no run-off on either side. Touching the rock or crossing the edge destroys the car.
8. **Viaduct Run**: several bridges, some very long, over rivers and over a deep valley; steel arches over the deck, no run-off beyond the parapets.
9. **Alpine Tunnels**: five tunnels of different lengths, some with bends inside, bored into grassy hills.

## Controls

### Keyboard

| Action | Keys |
| --- | --- |
| Steer | `Left` / `Right` or `A` / `D` |
| Accelerate | `Up` or `W` |
| Brake | `Down` or `S` |
| Toggle gear (LO/HI) | `Space` or `Shift` |
| Select LO / HI gear | `Z` / `X` |
| Pause / resume | `P` or `Esc` |
| Exit to menu (while paused) | `Q` |
| Mute | `M` |
| Day / night (title screen and Training) | `N` |
| Show / hide minimap | `T` |
| Mouse steering | `C` (then click the game to capture the pointer) |
| Menu | `Up` / `Down`, `Left` / `Right`, `Enter` |

### Touch

On-screen buttons appear automatically on touch devices: left, right, gas, brake, gear and pause. In menus, tap a row to start it; in the Training and Mode rows, tapping the left or right part of the row changes the value, and tapping the Time row switches between day and night.

### Gamepad, wheel and paddle shifters

Any browser-recognized gamepad or wheel is used automatically.

| Action | Input |
| --- | --- |
| Steer | Left stick axis or D-pad |
| Accelerate | `A` or right trigger |
| Brake | `B` or left trigger |
| Gear LO / HI | `LB` / `RB` (paddle shifters usually map here); `X` / `Y` toggle |
| Pause / start | `Start` |
| Exit to menu (while paused) | `B` |

Browsers expose gamepads only after a button press, and may block the Gamepad API inside sandboxed frames; in that case the game keeps working with keyboard and touch.

## Scoring (Arcade)

* Distance driven adds points continuously.
* Qualifying position adds `(9 - position) x 500`.
* Each position gained in the race adds 100.
* Finishing adds `(9 - final position) x 1000` plus 50 per second of time left.

The best score and the chosen driving mode are stored in the browser's `localStorage`.

## Technical notes

* Rendering: segment-based pseudo-3D projection on a 640x480 canvas, scaled to the viewport while keeping a 4:3 aspect ratio.
* Simulation: fixed 60 Hz timestep, independent of display refresh rate.
* Sound: engine and effects synthesized with the Web Audio API; audio starts after the first user input, as browsers require.
* Cornering model: target lateral drift = `k * |curve|^p * speed^2`, followed by the car through a first-order lag with time constant `tau`. The per-mode constants `k`, `p` and `tau` are in the `DRIVES` table near the top of the script (Easy uses `tau = 0`, i.e. no lag).
* Automatic pause when the tab or window loses focus.

Tracks are defined as compact lists of straights, curves, tunnel, river/bridge and mountain-side sections (`TRACKDEFS`); themes (`THEMES`) define sky, scenery layers and roadside objects.

## Known limitations

* The road has no elevation: the mountain wall, the precipice and the valley under the viaduct are drawn scenery on a flat road, not real 3D geometry. The precipice is shown as sea beyond the road edge, without a visible drop.
* Difficulty values (time limits, traffic speed, qualifying thresholds) were tuned with automated driving tests, not extensive human play, and may need adjustment.
* Rendering and audio were checked in a headless environment; behavior on specific browsers and devices should be verified.
* Night mode uses canvas compositing modes (`source-atop`, `destination-over`, `lighter`) on a 640x480 canvas; very old browsers may render it differently.

## Changelog

* **1.14**: Coast Road: the rock wall on the left now leans back about 12.5° from vertical and has a more uniform, layered rock look.
* **1.13**: Cascade Valley: waterfalls now descend from continuous, opaque mountain ranges along the valley (one long range where waterfalls are close together, a single range with a tapering start and end where they are isolated); removed the free-standing hills.
* **1.12**: Cascade Valley reworked: continuous river with banks and animated ripples that crosses under the road at four bridges; valley slopes with animated waterfalls and splash pools replace the free-standing falls; removed the vertical falls strip from the background mountains.
* **1.11**: Corner sequences of all nine circuits redesigned: about 65% of consecutive corners alternate side and 35% repeat the same side (mostly on the side of the first corner); two circuits (Dusk Desert, Pine Forest) now start with a left corner, the others with a right one. Fixed the minimap being mirrored left-right.
* **1.10**: Alpine Tunnels: no more patches of sky visible at the corners of the vaulted tunnel when entering and driving inside.
* **1.9**: Arched tunnels and hill portals apply to Alpine Tunnels only; Riviera Streets keeps its rectangular tunnel and building portal. Removed bright artefacts on the tunnel vault.
* **1.8**: Arched (vaulted) tunnel cross-section; on Alpine Tunnels each portal is set into a grassy hill with rock patches and pines instead of a box-like building.
* **1.7**: Mouse pointer hidden while driving; new explosion (flash, fireball, shockwave, debris, sparks, smoke, flames) and a deeper, longer explosion sound.
* **1.6**: Minimap with finish line, player and opponent markers (`T` to toggle).
* **1.5**: Three new circuits: Coast Road (rock wall and precipice, both lethal), Viaduct Run (long bridges over rivers and a valley), Alpine Tunnels (five tunnels).
* **1.4**: Night mode with headlight cone, starry sky and tail lights on other cars; the tunnel is now dark and lit by the headlights only; in Intermediate and Expert the car leaves the road progressively instead of at the very start of a bend. Tunnel rendering reworked: no more flicker of the entrance, a finite portal building, smooth shading of walls, ceiling and road.
* **1.3**: Easy / Intermediate / Expert driving modes.
* **1.2**: Three new circuits (Riviera Streets with tunnel, Cascade Valley with river, bridges and waterfalls, Pine Forest).
* **1.1**: Training mode, pause menu with exit, mobile layout and Gamepad API fixes.
* **1.0**: First release: Arcade mode, three circuits, keyboard, touch and gamepad input.

## License

Copyright (C) the authors of Pole Racer.

This program is free software: you can redistribute it and/or modify it under the terms of the **GNU Affero General Public License, version 3**, as published by the Free Software Foundation. This program is distributed in the hope that it will be useful, but **without any warranty**; without even the implied warranty of merchantability or fitness for a particular purpose. See the [GNU AGPL v3](https://www.gnu.org/licenses/agpl-3.0.html) for details.

Note that the AGPL also applies when the program is made available to users over a network: if you modify it and host it, you must offer those users the corresponding source code.
