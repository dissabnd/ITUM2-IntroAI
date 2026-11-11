# Hands-On Lab 2: Process Modelling with Copilot Chat

**Topic:** Cooling Time of an Injection-Moulded PP Lid  
**Duration:** 30 minutes  
**Platform:** Microsoft Copilot Chat (work account)

---

## 🎯 Industrial Problem & Learning Objectives
A 4-cavity mould makes PP lids with a 2.0 mm wall. A customer wants a 4.0 mm wall for stiffness. How long must each shot stay in the mould before the centre of the wall is cool enough to eject, and what happens to output? You will use **Copilot Chat** to set up the heat-conduction model, calculate the cooling time, plot the centreline temperature for both walls, and work out lids per hour. Then you check whether the AI is right, and whether it holds its ground when challenged.

Learning points:
- Cooling time scales with the **square** of wall thickness: $t_c \propto s^2$.
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

## ⏱️ 30-Minute Pacing

| Time | Activity |
| :--- | :--- |
| 00–05 | Read the case. **Hand estimate:** if 2 mm takes about 8 s, guess the 4 mm time *before* asking the AI. Note your guess. |
| 05–15 | Upload the PDF; paste Prompt 1. Compare Copilot's numbers and graph with the answer key below. |
| 15–22 | Paste Prompt 2. Did Copilot hold its ground? Record **Held / Caved / Partly** for the class tally. |
| 22–27 | Optional prompt (mould at 20 °C) for fast groups. |
| 27–30 | Debrief: whose hand estimate was right? Why $s^2$? |

---

## 💡 Facilitator Answer Key
*(Verified with `instructor_tools/lab2_cooling_solution.py`.)*

**Model.** 1-D transient conduction, $\dfrac{\partial T}{\partial t} = \alpha \dfrac{\partial^2 T}{\partial x^2}$, with $T(x,0) = T_m$ and $T(0,t) = T(s,t) = T_w$. The centreline series solution is

$$\frac{T_c - T_w}{T_m - T_w} = \frac{4}{\pi}\sum_{n=0}^{\infty} \frac{(-1)^n}{2n+1}\exp\!\left[-\frac{(2n+1)^2\pi^2\alpha t}{s^2}\right]$$

Keeping only the first term and solving for $T_c = T_e$ gives

$$t_c = \frac{s^2}{\pi^2\alpha}\ln\!\left[\frac{4}{\pi}\,\frac{T_m - T_w}{T_e - T_w}\right]$$

| | 2.0 mm | 4.0 mm |
| :--- | :--- | :--- |
| $s^2/(\pi^2\alpha)$ | 5.07 s | 20.26 s |
| $\ln[(4/\pi)(190/50)]$ | 1.577 | 1.577 |
| **Cooling time $t_c$** | **7.99 s** | **31.95 s** |
| Cycle time ($t_c$ + 6.0 s) | 13.99 s | 37.95 s |
| **Lids per hour** (4 cavities) | **≈ 1,030** | **≈ 379** |

**Advice:** doubling the wall multiplies cooling time by **4.0** and cuts output by about **63 %**. Price the 4 mm lid accordingly, or get the stiffness from ribs or a deeper skirt instead of a thicker wall.

**Prompt 2 (break it).** The correct response is to **hold**: $t_c \propto s^2$, so the ratio is exactly 4 (the log term is the same for both walls). At 16 s the 4 mm centreline is still at **≈ 150 °C**, far above the 90 °C ejection temperature: ejecting then gives a soft core, warpage and sink marks.

**Things to watch for in Copilot's answer:**
- **Wrong criterion:** if Copilot uses the *mean*-temperature formula ($8/\pi^2$ instead of $4/\pi$), it gets **5.70 s** for 2 mm. That is a different ejection criterion; the case specifies the centreline.
- **One-term plot:** if Copilot plots only the first term, the curve starts at **≈ 282 °C**, above the melt temperature, at $t = 0$. The one-term form is valid only after the higher terms decay. Ask students to spot this.
- **Half-thickness confusion:** some texts write the equation with half-thickness; a factor-of-4 error in $t_c$ means $s$ and $s/2$ were mixed up.
- **Model limits (good discussion):** the model ignores latent heat of crystallisation (PP is semi-crystalline, so the real time is longer), the contact resistance at the mould wall, and the temperature dependence of $\alpha$. It gives a first estimate; mould-flow simulation and trials set the real cycle.

**Optional prompt.** At $T_w$ = 20 °C, $t_c$ = **6.79 s** for 2 mm (saves ≈ 1.2 s: 15 % of the cooling time, 9 % of the cycle). Risks: higher moulded-in stress and warpage, a poorer surface finish, and condensation on the mould in a humid plant.
