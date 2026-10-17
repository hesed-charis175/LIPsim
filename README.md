# LIPsim

A terminal-based **Linear Inverted Pendulum (LIP)** simulator with a **PID controller**, written entirely in `Risk`.

## Note on the Code
This mini-project is a crash test for my programming language, which is still under development.
The current released version of the language (v0.1.0) can't fully support this.
The **next version** of the language will fully support and optimize everything written here (I'm working on it).

Check [Risk on My website](https://hesed-charis.vercel.app/projects/risk) for the logs of changes I already made.

---

## What it does
* **LIP Dynamics:** Models a pendulum on a cart, with friction on both the cart and the pole.
* **PID Control:** Computes the force on the cart needed to keep the pendulum upright. The P, I and D terms are shown live.
* **Terminal Display:** Renders the cart, the pole, the state, the PID output and a live angle-history chart in real time.
* **Live tuning:** Change the PID gains while it runs and watch how the response changes.

## How to run

### Prerequisites

- The `riskc` Risk compiler
- A terminal of at least **94 x 33** characters, with colour and Unicode support

### 1. Compile
```bash
riskc lipsim.risk -o pendulum -O3 -march=native
```

### 2. Run

```bash
./pendulum
```

## Controls

| Key | Action |
|---|---|
| `←` / `→` | Kick the pendulum left / right (adds a disturbance to its angular velocity) |
| `w` / `s` | Increase / decrease **Kp** (±5) |
| `e` / `d` | Increase / decrease **Kd** (±2) |
| `space` | Pause / resume |
| `r` | Reset the simulation (keeps your current gains) |
| `q`, `Esc` or `Ctrl+C` | Quit |

Kp and Kd can't go below 0. Ki is fixed at 12 and has no key binding.

## Things to try

The default gains are `Kp = 90`, `Ki = 12`, `Kd = 24`. The pendulum starts tilted at about 57°, close to the 60° failure limit, and the controller has to catch it.

- **Stabilise it:** Run with the defaults and watch the angle chart settle toward 0°.
- **Break it:** Drop Kp with `s` until the pendulum can't recover and falls. Press `r` to try again.
- **Remove the damping:** Lower Kd with `d` and watch the oscillations grow.
- **Disturb it:** Once it's stable, tap `←` / `→` and see how fast the controller recovers.

## Simulation details

| Parameter | Value |
|---|---|
| Cart mass | 1.0 kg |
| Pole mass | 0.2 kg |
| Pole length | 0.5 m |
| Gravity | 9.81 m/s² |
| Max force | ±60 N |
| Track limit | ±2.2 m |
| Failure angle | ±60° |
| Timestep | 0.0025 s (8 substeps per frame) |

The pendulum counts as crashed once it tilts past 60° in either direction. The cart stops at the track limits. The integral term is clamped to avoid windup.
