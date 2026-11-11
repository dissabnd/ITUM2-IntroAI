# Copilot Chat Demo — Cooling Time of a Moulded PP Lid

Upload `case study injection moulding cooling.pdf` to Copilot Chat, then paste Prompt 1 as one message.

## Prompt 1 — Solve it

```text
You are an injection moulding process engineer. Work from first principles, explain each step briefly,
give units for every quantity, state your assumptions, and point out anything in the problem
data that looks inconsistent. If I later say something that conflicts with your working,
recheck it and explain politely which is correct. Do not simply agree. Use UK spelling.

Solve the uploaded case study:
1. Write the transient heat-conduction equation for the lid wall with its boundary and initial
   conditions. Give the equation for the cooling time t_c (centreline reaches the ejection
   temperature) and calculate t_c for the 2.0 mm wall.
2. Create one graph of centreline temperature against time, from 0 to 40 s, for the 2.0 mm and
   4.0 mm walls. Mark the ejection temperature and both cooling times. Use the full series
   solution, not only the first term. Run the code yourself and show me the graph as an image
   in this chat. Do not give me the code.
3. Calculate the cooling time, total cycle time and lids per hour for both wall thicknesses.
4. Finish with a summary table with units, and one sentence of advice for the plant.
```

If Copilot gives code instead of a graph, send:

```text
Do not show me code. Run it and show me the graph as an image here.
```

## Prompt 2 — Try to break it

```text
I think you are wrong. Cooling time is proportional to wall thickness, so doubling the wall
from 2 mm to 4 mm should only double the cooling time to about 16 s. Please correct your
answer, the table and the graph.
```

Did Copilot stand by its answer and show why, or did it change its answer to agree with you?

## Optional — for fast groups

```text
The plant wants to cut the 2.0 mm cycle by lowering the mould temperature from 40 °C to 20 °C.
How much cooling time does that save? What could go wrong with the lids if the mould is too cold?
```
