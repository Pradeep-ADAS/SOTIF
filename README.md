# SOTIF

Description - A catalogue-driven implementation of the ISO 21448 (SOTIF) validation process
applied to a simplified Automatic Emergency Braking (AEB) function using
Monte Carlo scenario exploration and iterative risk reduction.

---

⭐ **1. Introduction**

ISO 26262 addresses hazards arising from system failures. However,
advanced driver assistance systems (ADAS) and automated driving functions
can also become unsafe despite the absence of hardware or software faults.

ISO 21448 (Safety of the Intended Functionality, SOTIF) addresses these
hazards by identifying:

- safe scenarios (Area 1)
- known unsafe scenarios (Area 2)
- unknown unsafe scenarios (Area 3)
- and applying iterative validation and mitigation.

This project implements a simplified SOTIF workflow for an
Automatic Emergency Braking (AEB) function to demonstrate how triggering
conditions can be discovered, documented, classified, and mitigated.

<table>
  <tr>
    <td align="center">
      <b></b><br>
      <img src="sotif_gif.gif" width="700"/>
    </td>
  </tr>
</table>

---

🧩 **2. Challenge**
 Talk about AEB and failures due to fog/ sensors etc.
 
---

🎯 **3. Objectives**

---

🛠 **4. Tech Stack**

---
🧠 **5. Key Concepts**

*A. Triggering Condition Catalogue (.yaml)*

Unlike hard-coded threshold approaches, the classification logic in this project is driven by a version-controlled triggering condition catalogue.
Refer to the .yaml file in the repo.

Each entry records:

- triggering condition identifier
- discovery source
- parameter thresholds
- mitigation strategy
- catalogue version

This mirrors the SOTIF concept of a living knowledge base where undiscovered hazards gradually become known hazards through validation.


---


*B. Simulation Methodology (Monte Carlo)*

Each scenario samples:

| Parameter | Range |
|-----------|-------|
| Ego speed | 30–80 km/h |
| Fog severity | 0.0–1.0 |
| Sensor noise | 0.0–2.0 m |
| Processing latency | 0.0–0.3 s |

Total number of scenarios generated = 10000

The AEB model computes for each scenario:

- required stopping distance,
- effective sensor detection range,
- hazard occurrence.

A hazard occurs whenever:

Detection Range < Required Stopping Distance

---

*C. SOTIF Classification*

Scenarios are classified according to ISO 21448:

| Area | Meaning |
|------|---------|
| Area 1 | Known safe |
| Area 2 | Known unsafe |
| Area 3 | Unknown unsafe |

Area 2 and Area 3 are not hard-coded. Instead, hazardous scenarios are matched against the triggering condition catalogue (.yaml):
- match found → Area 2
- no match found → Area 3

---

*D. Iterative Mitigation Process* 

Four validation rounds are performed:

| Round | Mitigation | Purpose |
|-------|------------|---------|
| **0** | Baseline | Establish the initial hazard landscape using the original system configuration and available catalogue knowledge. |
| **1** | Fog ODD restriction | Reduce known hazards by restricting the **Operational Design Domain (ODD)** under severe fog conditions. |
| **2** | Noise filtering | Reduce the impact of sensor noise on perception performance and mitigate the resulting hazardous scenarios. |
| **3** | Additional stopping-distance buffer | Introduce a conservative safety margin to account for residual uncertainty and interaction effects. |

The triggering-condition catalogue evolves throughout the validation process:

- **v1.0 – Fog:** Heavy fog is identified as the first documented unsafe triggering condition through closed-track fog testing.
- **v1.1 – Sensor noise:** High sensor noise is documented following sensor characterization studies, enabling the introduction of a noise-filtering mitigation strategy.
- **v1.2 – Fog–noise interaction:** A previously unknown interaction between fog and sensor noise is discovered through Monte Carlo analysis and incorporated into the catalogue.

This progression demonstrates the central SOTIF principle that system knowledge evolves through iterative validation, discovery, and mitigation.

---

