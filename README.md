# PHAGO — White Blood Cell Swarm

A playable build of `white_blood_cell_game_design.md` and `gemini-code-1788032529942.md`.
Open **`index.html`** in any modern browser — no server, no build step, no dependencies.

## The premise

You do not control cells. You control a **cytokine beacon** (the cursor); the swarm reads the
gradient and chases it. And the pathogens do not chase *you* — every species carries its own
objective against the **host**. You are an interceptor, and the clock is the host's body.

## Controls

| Input | Action |
| :--- | :--- |
| Mouse | Move the beacon — the swarm follows |
| Hold LMB | Compress into a dense ramming ball (formation radius 50) |
| Hold RMB | Disperse into a wide filter net (radius 300) |
| `Q` | Opsonin volley — tags targets, strips capsules, doubles digestion, **neutralises pyroptosis** |
| `E` | Agglutination volley — cross-links pathogens into slow clumps |
| `SPACE` | Oxidative burst — reactive-oxygen AoE that costs 16% of your own integrity |
| `F` | Superoxide Spray (Neutrophil T2) — also burns fungal mats |
| `R` | Giant Cell Fusion (Macrophage keystone) |
| `T` | Granzyme Death Sentence (T-cell keystone) — or clear a parasitised red cell |
| `C` | Toggle M1 / M2 macrophage stance |
| `P` | Pause |

## Losing

**Host integrity** is the primary meter and the primary fail state. It drains from every
erythrocyte lysed, every breached wall segment, and every fungal mat left standing, and it
repairs during the resolution phase between waves. Losing every leukocyte also ends the run,
but while any node survives the marrow keeps producing more.

Erythrocyte count feeds back into you: **O₂** scales swarm speed, so letting haemolysis run
unchecked slows the swarm down exactly when it can least afford it.

## Pathogen objectives (§4 mechanics matrix)

| Species | Objective | Mechanic |
| :--- | :--- | :--- |
| **Streptococcus** | red cells | Streptolysin haemolysis — the baseline drain on the host |
| **Salmonella** | red cells | **Pyroptosis** — detonates inside the phagocyte that swallows it. Opsonise it first and the blast is neutralised. Marked by a pulsing orange ring. |
| **Merozoite** (*Plasmodium*) | red cells | **Intracellular parasitism** — evades leukocytes, invades an erythrocyte, incubates, then bursts into two daughters. Left alone it grows exponentially. |
| **Staph. aureus** | red cells | **α-haemolysin pores** — ranged shots that permanently cut a node's *maximum* HP. Encapsulated, so it must be tagged before it can be eaten. |
| **Pseudomonas** | **the swarm** | **T3SS needle** — latches onto a node, suppresses mitosis and drains banked biomass until killed |
| **Clostridium** | vessel wall | **Tissue liquefaction** — chews endothelial segments into breaches and leaves necrotic terrain that stops them healing. Breached wall gives no Marginate grip. |
| **Aspergillus** | open lumen | **Occlusion** — roots down and grows a mycelial mat that physically blocks the swarm and drains the host until chewed out |
| **Biofilm Colony** (elite) | red cells | Buds daughter cocci; below 45% HP it **enrages and vents LPS**, and on death floods the area with endotoxin clouds that damage nodes and red cells alike |

Countering the parasite is a genuine trade: nodes touching a parasitised erythrocyte destroy it
(collateral CD8 clearance) for a fraction of the host cost of letting it burst. `T` does the
same thing cleanly at range.

## Design doc → code

| Doc mechanic | Implementation |
| :--- | :--- |
| Phagocytosis + **mass gating** | `contactPass()` sums engulf power from nodes in contact plus, at 35%, those within pseudopodia reach. Under the threshold nothing happens and the target shows a **MASS have/need** readout; over it, an encirclement ring fills. Small prey is swallowed on touch. |
| Damage ↔ mass coupling | `effMass()` scales with remaining HP, so contact lysis *softens* big prey into something the swarm can encircle. Contact damage scales sub-linearly with node count, so smothering never replaces eating. |
| Digestion phase | The devouring node carries a vacuole: −42% speed, biomass paid over the digest, and a hit can rupture it — freeing the pathogen **enraged**. |
| Opsonisation | Strips armour and capsule, ×2 digestion, +25% biomass, homing priority, and neutralises pyroptosis. |
| Agglutination | Bound pathogens spring together, lose speed by cluster size, and eating one chains damage through the clump (×1.6 biomass). |
| NETosis | A dying node spins a chromatin web that roots pathogens for 4s under acid burn. |
| Memory Protocol | Five kills of a species imprints a permanent damage bonus against it. |

## Hybrid synergies (§5)

These are not upgrade cards. They are emergent properties of a multi-class build: invest in two
lineages far enough and the synergy switches itself on with a full-screen announcement and a
permanent chip in the HUD. Level-up cards that would complete one are flagged with a gold
**⚡ Completes …** badge, so you can steer toward them deliberately.

| Synergy | Lineages | Requirement | Effect |
| :--- | :--- | :--- | :--- |
| **Opsonin Feast** | Neutrophil + Macrophage | NETosis, 3 Macrophage ranks | Prey pinned in a chromatin web yields **double biomass** and digests with **no speed penalty** — the vacuole drag is gone |
| **Antigen Presentation** | Macrophage + T-Lymphocyte | 3 Macrophage + 3 T-Lymphocyte ranks | Finishing digestion of a heavyweight displays its antigen: **×2.2 damage against that entire species**, swarm-wide, for 22s. Every member of the species is ringed with MHC-I marks while it lasts |
| **Targeted Lysis Shock** | Neutrophil + T-Lymphocyte | NETosis + Granzyme Death Sentence | Every apoptotic detonation seeds a **mini-NET trap** on each collateral target caught in the blast — one execution can web a dozen pathogens at once |

One deliberate liberty: the doc specifies Antigen Presentation triggers on an **elite**, and the
only elite species is the Biofilm Colony, which would make the synergy nearly unreachable. It
also triggers on any heavyweight (mass ≥ 5): Pseudomonas, Clostridium and Aspergillus.

## Balance

Tuned against an instrumented sweep rather than by feel. The game exposes a debug hook,
`G.turbo`, which runs `stepSim()` more than once per frame without enlarging `dt`, so physics and
collision stay identical. A scripted autopilot drove ~12 full runs per iteration at speed and
recorded per-wave host loss, attributed by cause (haemolysis / parasite bursts / wall breaches /
fungal mats), plus wave duration, parasite population and swarm low-water mark.

Where it landed (12 runs, near-optimal autopilot):

| Waves | Host cost/wave | Character |
| :--- | :--- | :--- |
| 1–7 | 2 → 6 | Learning curve; resolution regen roughly refunds it |
| 8–14 | 11 → 18 | Net attrition, host drifts 90% → 40% |
| 15+ | 21 → 32 | Collapse |

All 12 runs reached wave 15–17, and **11 of 12 ended by host failure** rather than swarm wipe —
host integrity is the fail state the design intends. A human plays worse than the autopilot, so
expect to lose somewhere around waves 10–14.

Four things the sweep found that were not visible from playing:

- **Encirclement was capped by geometry.** Engulf power counted only nodes in physical contact,
  and only ~9 nodes fit around a large target — so heavy prey could never be eaten no matter how
  large the swarm grew, and the biofilm reliably wiped a full 21-node swarm. Nodes just outside
  contact now contribute at 35% (pseudopodia reach).
- **Clostridium's necrotic terrain caused 74% of all node deaths.** Zones spawned ~0.7/s, lasted
  6s and stacked to ~36 dps. Now rate-limited, non-overlapping, and much weaker per zone: it
  exists to stop the wall closing, not to delete leukocytes.
- **Breach drain was linear and unrecoverable.** Every breached segment drained the host forever;
  breaches accumulated faster than they healed. Drain is now sub-linear in breach count, in-combat
  repair is faster, and **parking the swarm on a wound accelerates re-endothelialisation** — so
  defending the wall is a real action.
- **Parasitaemia had no equilibrium.** Merozoites reproduced at or above replacement forever, so
  waves containing them could not end. Free merozoites now perish if they fail to find a host
  cell, daughters taper with population, and clearing a parasitised cell is fast and decisive.

Per-wave species ceilings (`CAP` in `buildWave`) stop any one nasty species filling a whole wave,
and the marrow rubber-bands: regeneration speeds up sharply when the swarm is far below capacity,
so a bad wave is a setback rather than a spiral.

## Structure

Everything is in `index.html`, sectioned by comment banner: utilities, state and derived stats,
species table, upgrade tree, entity factories, simulation, projectiles/FX, abilities, waves,
rendering, UI, input, main loop. `recalc()` is the single place upgrade stacks become gameplay
numbers — tune there first. Per-species behaviour lives in the `PATHO` table plus the objective
blocks at the end of `updatePathogens()`.
