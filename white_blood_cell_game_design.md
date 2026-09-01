# White Blood Cell Swarm: Game Design Document & Systems Guide

## 1. Executive Summary & Working Titles

In this micro-scale action game, the player commands a dynamic, fluid swarm of white blood cells defending an organism from escalating pathogen invasions. Gameplay bridges biological accuracy with fast-paced swarm control, area-of-effect crowd dynamics, and tactical class specialization.

### Working Title Candidates

* **Punchy & Action-Oriented:**
  * *Phagocyte* (or *PHAGO*)
  * *Host Defense*
  * *Cell Shock*
  * *Cell Swarm*
  * *Whiteout*
  * *Immunity Protocol*
  * *Vessel Breach*
* **Biological & Sci-Fi:**
  * *Leukocyte*
  * *Antigen*
  * *Adaptive Defense*
  * *Biomass Swarm*
  * *Cytoplasmic*
  * *The Engulfing*
  * *Vascular*
* **Clever & Playful:**
  * *Eat the Sick*
  * *Bad Blood*
  * *Swarm & Destroy*
  * *Fever Pitch*
  * *Gulp Culture*
  * *Marrow Marauders*
* **Abstract & Atmospheric:**
  * *In Vitro*
  * *The Living Shield*
  * *Endless Host*
  * *Cleanse*
  * *Hemosphere*

---

## 2. Biological Foundations

The human immune system pacifies and eliminates threats through a coordinated sequence bridging the innate system (immediate, non-specific responders) and the adaptive system (targeted, memory-driven specialists).

```
[ Pathogen Infiltration ]
           │
           ├─► 1. Chemical Warning & Chemotaxis (Cytokines guide cells)
           │
           ├─► 2. Opsonization & Tagging (Antibodies & Complement coat target)
           │
           ├─► 3. Agglutination (Antibodies cross-link pathogens into clumps)
           │
           ├─► 4. Phagocytosis (Engulfment -> Phagolysosome -> Enzymatic Lysis)
           │
           ├─► 5. Cytotoxic Action (Perforin/Granzyme inject apoptosis into infected cells)
           │
           └─► 6. Complement Cascade (MAC punches physical pores -> Osmotic rupture)
```

### Core Immune Mechanisms
1. **Neutralization:** Antibodies bind surface antigens, physically blocking pathogens from entering host cells.
2. **Agglutination:** Multi-binding antibodies clump thousands of bacteria or viral particles into immobile clusters.
3. **Opsonization:** Antibodies and complement proteins tag pathogens, drastically increasing phagocytic efficiency.
4. **Phagocytosis:** Phagocytes extend pseudopodia around targets, encasing them in a phagosome that fuses with lysosomes for enzymatic digestion.
5. **Cytotoxic Killing:** CD8+ T-cells and Natural Killer cells punch pores (perforin) and inject granzymes to trigger programmed cell suicide (apoptosis).
6. **Complement System:** A chemical cascade forming the Membrane Attack Complex (MAC), drilling pores into target membranes until osmotic pressure causes lysis.

---

## 3. Translation into Game Mechanics

| Biological Phenomenon | Mechanical Translation | Gameplay Role |
| :--- | :--- | :--- |
| **Phagocytosis** | Engulfment & Mass Scale Check | Primary kill mechanic, XP/Biomass gathering |
| **Opsonization** | Target Tagging & Defense Strip | Armor-shredding, AI swarm targeting priority |
| **Agglutination** | Webbing / Clumping Tether | Crowd control, grouping enemies for AoE |
| **Chemotaxis** | Pheromone / Beacon Steering | Swarm formation manipulation (Spread vs. Dense) |
| **Reactive Oxygen Species** | Oxidative Flare / Acid Pool | High-risk emergency burst AoE at self-HP cost |
| **Complement System (MAC)** | Orbiting Turrets / Membrane Breach | Percentage-based true damage & boss armor penetration |

### Deep-Dive System Designs

#### A. The Phagocytosis Devour Loop
* **Mass Gating:** Enemies have mass thresholds. Small bacteria (*Cocci*) are consumed instantly on touch. Large threats (*Spirilla*, parasites) require encirclement by multiple cell nodes.
* **Digestion Phase:** Engulfed enemies are stored in an internal vacuole during breakdown:
  * Swarm speed drops 30–50% per active digestion.
  * Yields continuous **Biomass / ATP**.
  * Premature damage disrupts the vacuole, freeing the pathogen with enrage status.

#### B. Opsonization & Target Acquisition
* Unmarked enemies possess protective capsules that cause player cells to slide off or suffer contact damage.
* Coating enemies with opsonin projectiles tags them: strips armor, grants AI cell sub-units auto-leash priority, and doubles digestion speed.

#### C. Agglutination Nets (Crowd Control)
* Firing Y-shaped antibody volleys binds colliding enemies together.
* Enemies welded together lose movement speed dynamically based on cluster size.
* Digesting an agglutinated clump triggers combo multipliers and chain reactions.

#### D. Chemotaxis & Swarm Dynamics
* Indirect control model: The player guides a cytokine signal beacon.
* High beacon concentration compresses the swarm into a dense ramming ball.
* Dilute signal spreads the swarm into an expansive filter net for clearing micro-swarms.

---

## 4. Leukocyte Upgrade & Lineage Tree

```
                       [ STEM CELL CORE ]
                               │
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
     [ MYELOID LINEAGE ]                  [ LYMPHOID LINEAGE ]
   (Brute Force / Mass)                  (Precision / Tactics)
            │                                     │
     ┌──────┴──────┐                              │
     ▼             ▼                              ▼
[NEUTROPHIL]  [MACROPHAGE]                    [T-LYMPHOCYTE]
(Swarm Burn)  (Titan Devour)                 (Target Assassin)
```

### Tier 0: Stem Cell Core (Universal Passives)
* **Mitosis Rate:** Increases baseline node reproduction; fallen nodes regenerate faster upon consuming pathogens.
* **Chemotactic Sensitivity:** Boosts base glide speed and reduces viscosity drag across vascular fluids.
* **Membrane Elasticity:** Increases swarm deformability, allowing tight squeezing through capillaries without scattering.

---

### Branch 1: The Neutrophil Path (*High Speed, Volatile Acid, Sacrificial CC*)
*Fast, aggressive first-responders built around damage-over-time acid trails and tactical self-sacrifice.*

* **Tier 1: Frontline Chemotaxis**
  * *Marginate:* Clinging to blood vessel walls grants a 25% sprint boost.
  * *Granule Vent:* Passive leakage of granule enzymes deals continuous contact burn to bordering pathogens.
* **Tier 2: The Respiratory Burst**
  * *Superoxide Spray (Active):* Vents hydrogen peroxide ($H_2O_2$) in a forward cone, stripping enemy shields.
  * *Degranulation Shock:* Absorbing an enemy detonates an enzymatic shockwave that staggers adjacent threats.
* **Tier 3: NETosis (Keystone Ability)**
  * *Neutrophil Extracellular Trap (NET):* When a cell node's HP is depleted, it undergoes programmed rupture, spinning a web of sticky chromatin fibers that locks targets in place for 4 seconds under heavy acid burn.

---

### Branch 2: The Macrophage Path (*Colossal Mass, Tanking, Vacuum Ingestion*)
*Long-lived heavyweights designed to anchor the battlefield, swallow massive threats, and process vast biomass.*

* **Tier 1: Massive Phagosome**
  * *Deep Ingestion:* Increases maximum absorbable enemy mass threshold by 50%.
  * *Chitinase Breakdown:* Decreases digestion duration by 30%, converting captured matter directly into swarm integrity.
* **Tier 2: Scavenger Morphology**
  * *M1 Inflammatory Stance (Toggle):* Increases collision knockback and armor, sacrificing drift speed.
  * *M2 Resolving Stance (Toggle):* Grants 40% damage resistance and emits an aura that regenerates surrounding allied nodes.
* **Tier 3: Giant Cell Fusion (Keystone Ability)**
  * *Multinucleated Titan:* Activates to fuse 10+ sub-cells into an enormous apex macrophage. Immune to crowd control, drags bosses into a crushing digestive vortex, and strips outer membranes.

---

### Branch 3: The T-Cell Path (*Surgical Assassination, Critical Synergy, Antigen Memory*)
*Tactical specialists utilizing antibody coordination, critical strikes, and programmed death sentences.*

* **Tier 1: Antigen Recognition**
  * *Receptor Affinity:* Tagged enemies become high-priority targets; sub-cells gain 35% movement homing toward them.
  * *Perforin Needle:* Basic collisions bypass 50% of pathogen armor plates.
* **Tier 2: Helper Orchestration (CD4+)**
  * *Interleukin Cascade:* Tagging an elite pathogen broadcasts a beacon granting nearby nodes 20% attack and devour speed.
  * *Memory Protocol:* Defeating 5 specimens of an enemy archetype grants a permanent 15% damage bonus against that species for the run.
* **Tier 3: Cytotoxic Execution (CD8+ Keystone Ability)**
  * *Granzyme Death Sentence:* Latches onto an elite target and injects apoptotic enzymes, starting a 3-second internal countdown. When complete, the pathogen self-destructs and unleashes a shockwave that detonates adjacent marked targets.

---

## 5. Multi-Class Hybrid Synergies

| Synergy Combination | Title | Mechanical Effect |
| :--- | :--- | :--- |
| **Neutrophil + Macrophage** | **Opsonin Feast** | Pathogens pinned inside a NET web grant double biomass and zero digest slowdown when devoured by a Macrophage. |
| **Macrophage + T-Cell** | **Antigen Presentation** | When a Macrophage digests an elite pathogen, it displays that enemy's antigen, granting immediate global critical strike bonuses against that species. |
| **Neutrophil + T-Cell** | **Targeted Lysis Shock** | Cytotoxic countdown detonations automatically spawn mini-NET traps on every collateral target caught in the explosion. |
