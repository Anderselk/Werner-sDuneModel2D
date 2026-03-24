# Werner-sDuneModel2D
2D cellular automaton simulation of sand dune formation (Werner model) exploring ripple scaling and wind-driven morphology.
# Werner Dune Model (2D)

## Overview
Sand dunes are a classic example of **emergent complexity**, where simple local interactions between individual grains produce large-scale, organized structures such as ripples and dunes.

This project implements the **Werner (1995) cellular automaton model** for aeolian (wind-driven) sand transport in 2D. Despite its simplicity, the model reproduces key qualitative behaviors observed in real dune fields, including:
- Ripple formation
- Dune migration
- Morphological transitions under changing wind conditions

---

## Research Questions

This project investigates two core questions:

1. **Validation:**  
   Does ripple wavelength scale with saltation length \( L_s \) as expected theoretically?

2. **Wind Morphology:**  
   How does wind variability (direction and randomness) influence emergent dune structure?

---

## Model Description

The simulation runs on a **2D periodic lattice** of size \( L \times L \), where each cell stores an integer height representing sand slabs.

Each timestep consists of four rules:

### 1. Erosion
- Sand is removed with probability \( p_{erosion} \)
- Only occurs outside shadow zones

### 2. Saltation
- Transport of sand grains a fixed distance \( L_s \) in the wind direction
- Implements the dominant physical transport mechanism

### 3. Deposition & Shadow Zone (Key Mechanism)
- Deposition depends on whether a site is **shadowed** by upstream dunes
- Shadow condition:
  
  \[
  h_{upwind} > k \cdot \theta_s + h_{dest}
  \]

- Behavior:
  - In shadow: deposit with probability 1
  - Outside: deposit with probability \( p_{dep} \)

### 4. Avalanche
- Enforces slope stability
- Sand redistributes if height differences exceed a threshold

---

## Wind Regimes

Four wind environments are studied:

- **Unidirectional:** constant wind → elongated dunes  
- **Bidirectional:** alternating wind → symmetric ridges  
- **Oblique:** diagonal wind → rotated dune structures  
- **Random (0–360°):** isotropic wind → clustered dune peaks  

---

## Methodology

### Simulation Parameters
- Grid size: `L = 128`
- Steps: `600`
- Erosion probability: `0.5`
- Deposition probability: `0.6`
- Shadow angle: `0.2`
- Slope threshold: `4`

### Wavelength Measurement
- FFT applied to each row
- Power spectrum averaged
- Peak frequency → dominant wavelength

---

## Results

### 1. Ripple Scaling (Validation)
- Ripple wavelength increases with saltation length \( L_s \)
- Observed relationship:

\[
\lambda \sim L_s^\alpha
\]

- Slight deviation from linear scaling at large \( L_s \) due to over-erosion effects

---

### 2. Wind Regime Effects

- **Unidirectional:**  
  Produces elongated, migrating dunes

- **Bidirectional:**  
  Creates highly ordered, symmetric patterns

- **Oblique:**  
  Rotated dunes with increased effective transport distance (\( \sqrt{2} \) effect)

- **Random Wind:**  
  Produces isotropic clustering and dune “peaks” via constructive interference

---

### 3. Emergence Behavior
- System evolves from flat → disordered → organized ripples
- Structure emerges within ~20–100 steps

---

## Key Insights

- **Shadow zones are critical** for dune formation  
- Wind directionality strongly controls morphology  
- Small implementation details (e.g., vectorization order) can introduce subtle biases  
- Emergent structure depends more on **mechanism correctness** than parameter tuning  

---

## Code Structure

### Core Simulation
- `initialize_surface`
- `compute_shadow_zone`
- `werner_step`
- `run_werner`

### Wind Variants
- `run_werner_bidirectional`
- `run_werner_random_wind`

### Analysis
- `measure_ripple_wavelength`

### Visualization
- Height maps
- Power spectra
- Surface width evolution

---

## Installation

```bash
pip install numpy matplotlib scipy
