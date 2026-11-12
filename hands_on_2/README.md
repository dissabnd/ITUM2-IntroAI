# Hands-On Lab 2: Process Modelling with Copilot Chat

**Topic:** Cooling Time of an Injection-Moulded PP Lid  
**Duration:** 30 minutes  
**Platform:** Microsoft Copilot Chat (work account)

---

## 🎯 Industrial Problem & Learning Objectives
A 4-cavity mould makes PP lids with a 2.0 mm wall. A customer wants a 4.0 mm wall for stiffness. How long must each shot stay in the mould before the centre of the wall is cool enough to eject, and what happens to output? You will use **Copilot Chat** to set up the heat-conduction model, calculate the cooling time, plot the centreline temperature for both walls, and work out lids per hour. Then you check whether the AI is right, and whether it holds its ground when challenged.

Learning points:
- Cooling time scales with the **square** of wall thickness: *t*<sub>c</sub> ∝ *s*² (doubling wall thickness quadruples cooling time).
- Wall thickness is the main lever on cycle time and cost, not a cosmetic choice.
- A confident AI answer must be checked against the physics, and a correct one should not change because the user sounds sure.

---

## Files

| File | Description |
| :--- | :--- |
| `case study injection moulding cooling.pdf` | The problem: wall sketch, given data and the four tasks |
| `prompts.md` | Prompt 1 solves the problem and plots; Prompt 2 tries to break the AI; optional what-if |
| `solution_centreline_temperature.png` | Facilitator only: correct graph to compare with Copilot's |

---

## ⏱️ Recommended Pacing

| Time | Activity |
| :--- | :--- |
| 00–05 | Read the case. **Hand estimate:** if 2 mm takes about 8 s, guess the 4 mm time *before* asking the AI. Note your guess. |
| 05–15 | Upload the PDF; paste Prompt 1. Compare Copilot's numbers and graph with the answer key below. |
| 15–22 | Paste Prompt 2. Did Copilot hold its ground? Record **Held / Caved / Partly** for the class tally. |
| 22–27 | Optional prompt (mould at 20 °C) for fast groups. |
| 27–30 | Debrief: whose hand estimate was right? Why *s*²? |

---

## 💡 Facilitator Answer Key
*(Verified with `instructor_tools/lab2_cooling_solution.py`.)*

### 1. Governing Heat Conduction Model
1-D transient heat conduction through the part wall thickness *s*:

```text
 ∂T        ∂²T
──── = α · ────
 ∂t        ∂x²
```

- **Initial condition:** *T*(*x*, 0) = *T*<sub>m</sub> (melt temperature = 230 °C)  
- **Boundary conditions:** *T*(0, *t*) = *T*(*s*, *t*) = *T*<sub>w</sub> (mould wall temperature = 40 °C)  

---

### 2. Centreline Series Solution
The exact analytical series solution for temperature at the centreline (*x* = *s*/2):

```text
Tc - Tw     4    ∞   (-1)ⁿ            ⎡ -(2n+1)² · π² · α · t ⎤
─────── = ─── ·  Σ  ────── · exp      ⎢ ───────────────────── ⎥
Tm - Tw     π   n=0 2n + 1            ⎣          s²           ⎦
```

---

### 3. Cooling Time Formula (First-Term Approximation)
Keeping only the dominant first term (*n* = 0) and solving for when the centreline reaches ejection temperature (*T*<sub>c</sub> = *T*<sub>e</sub> = 90 °C):

```text
       s²         ⎡  4   ⎛ Tm - Tw ⎞ ⎤
tc = ────── · ln  ⎢ ─── · ⎜ ─────── ⎟ ⎥
      π²·α        ⎣  π   ⎝ Te - Tw ⎠ ⎦
```

Where:
- Temperature ratio: (230 − 40) / (90 − 40) = 190 / 50 = 3.80
- Logarithmic factor: ln[(4/π) · 3.80] = ln[4.838] ≈ **1.577**
- Time constant factor: *s*² / (π² · α)

---

### 4. Summary Table & Cycle Comparison

| Parameter / Metric | Formula / Basis | 2.0 mm wall | 4.0 mm wall |
| :--- | :--- | :--- | :--- |
| **Diffusion time factor** | *s*² / (π² · α) | 5.07 s | 20.26 s |
| **Temperature ratio term** | ln[(4/π) · (190 / 50)] | 1.577 | 1.577 |
| **Cooling time (*t*<sub>c</sub>)** | *t*<sub>c</sub> = 5.07 s × 1.577 | **7.99 s** | **31.95 s** |
| **Cycle time** | *t*<sub>c</sub> + 6.0 s dry cycle | 13.99 s | 37.95 s |
| **Production output** | (3600 / cycle) × 4 cavities | **≈ 1,030 lids/h** | **≈ 379 lids/h** |

---

### 5. Discussion & Facilitator Notes

- **Advice for the plant:** Doubling the wall multiplies cooling time by **4.0** and cuts output by about **63 %**. Price the 4 mm lid accordingly, or achieve required stiffness using reinforcing ribs or a deeper skirt rather than increasing wall thickness.
- **Prompt 2 (Break it):** The correct response is to **hold**: *t*<sub>c</sub> ∝ *s*², so the ratio is exactly 4 (the logarithmic term is identical for both walls). At 16 s the 4 mm centreline is still at **≈ 150 °C**, far above the 90 °C ejection temperature: ejecting prematurely results in a soft core, severe warpage, and sink marks.

**Things to watch for in Copilot's answer:**
- **Wrong criterion:** If Copilot uses the *mean*-temperature formula (8/π² instead of 4/π), it gets **5.70 s** for 2 mm. That is a different ejection criterion; the case explicitly specifies the centreline temperature.
- **One-term plot:** If Copilot plots only the first term, the curve starts at **≈ 282 °C** at *t* = 0 (above the melt temperature). The single-term approximation is valid only after the higher-order terms decay. Ask students to spot this artifact.
- **Half-thickness confusion:** Some textbooks define the heat equation using half-thickness (*h* = *s*/2); a factor-of-4 error in *t*<sub>c</sub> indicates *s* and *s*/2 were conflated.
- **Model limits:** The analytical model neglects latent heat of crystallisation (polypropylene is semi-crystalline, so real cooling is slower), thermal contact resistance at the mould wall, and the temperature dependence of thermal diffusivity α. It provides a first-order engineering estimate; mould-filling simulation and trials calibrate the production cycle.

**Optional prompt:** At mould temperature *T*<sub>w</sub> = 20 °C, cooling time *t*<sub>c</sub> = **6.79 s** for 2 mm (saving ≈ 1.2 s: 15 % of cooling time, 9 % of the cycle). Risks: higher moulded-in residual stress and warpage, compromised surface finish/gloss, and atmospheric condensation (sweating) on the mould in humid environments.
