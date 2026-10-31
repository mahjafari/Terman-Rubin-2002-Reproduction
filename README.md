# Terman-Rubin-2002-Reproduction

A reproduction of results from:

> Terman D, Rubin JE, Yew AC, Wilson CJ (2002). **Activity patterns in a model for the subthalamopallidal network of the basal ganglia.** *Journal of Neuroscience* 22(7):2963–2976.

## Overview

The paper presents a conductance-based network model of the subthalamic nucleus (STN) and the external segment of the globus pallidus (GPe) in the indirect pathway of the basal ganglia. Depending on the connectivity architecture and the synaptic conductances, the network produces three classes of sustained firing patterns:

- **Clustering**: subsets of cells become tightly synchronized, and clusters alternate firing.
- **Propagating waves**: solitary pulses of activity travel across the network.
- **Irregular / uncorrelated spiking**: sparse or continuous firing with little regularity or correlation.

Each pattern can occur continuously or in discrete episodes. Stronger striatal input to GPe combined with weaker intra-GPe inhibition can switch the network from irregular to rhythmic activity, which the authors propose as a possible mechanism for the correlated oscillations seen after dopamine depletion in Parkinson's disease.

## What this repository reproduces

This repository contains code for **Figure 5** only: activity patterns in the structured, sparsely connected STN–GPe network (weak, episodic and continuous clustering). The other figures in the paper are not reproduced here.

## Model summary

**STN cell** (single compartment): leak, K, Na, low-threshold T-type Ca, high-threshold Ca, and Ca-activated AHP currents, plus synaptic input from GPe.

```
Cm dv/dt = -IL - IK - INa - IT - ICa - IAHP - IG→S
```

**GPe cell**: same form, with a simplified T current, plus excitatory input from STN, inhibitory input from other GPe cells, and a constant applied current `Iapp` representing striatal inhibition.

```
Cm dv/dt = -IL - IK - INa - IT - ICa - IAHP - IS→G - IG→G + Iapp
```

**Synapses**: first-order kinetics, `ds/dt = α H(v_pre − θ_g)(1 − s) − β s`, with `H(v) = 1 / (1 + exp(−(v − θ_g^H) / σ_g^H))`.

Full parameter values are in Tables 1 (STN) and 2 (GPe) of the paper. Networks have 8–20 cells of each type.

### Structured, sparsely connected architecture (Figure 5)

- Each GPe neuron inhibits its two immediate GPe neighbors and two STN neurons, skipping the three nearest.
- Each STN cell excites only the nearest, in-register GPe cell.
- Spatially periodic boundary conditions.
- Shared settings: `gG→S = 4.5 nS/µm²`, `Iapp = −1.0 pA/µm²`, `vG→G = −85 mV`, `β = 0.04 ms⁻¹`.

| Pattern | `gG→G` (nS/µm²) | `gS→G` (nS/µm²) |
|---|---|---|
| Weak, irregular clustering | 0.06 | 0.2 |
| Episodic clustered oscillations | 0.06 | 0.56 |
| Continuous clustering | 0.06 | 0.72 |

## Requirements

- Python 3.9 or newer
- Packages listed in [`requirements.txt`](requirements.txt) (NumPy, SciPy, Matplotlib)

The original simulations in the paper were run in [XPPAUT](http://www.math.pitt.edu/~bard/xpp/xpp.html).

## Installation

```bash
git clone https://github.com/mahjafari/Terman-Rubin-2002-Reproduction.git
cd Terman-Rubin-2002-Reproduction
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Usage

Run the script in `Figure 5/` to simulate the three clustering regimes and plot the voltage traces and raster-style panels corresponding to Figure 5c and 5d:

```bash
cd "Figure 5"
python <script_name>.py
```

Replace `<script_name>.py` with the main script in that folder.

## Repository structure

```
.
├── Figure 5/     # Code for the Figure 5 reproduction
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Reference

Terman D, Rubin JE, Yew AC, Wilson CJ (2002). Activity patterns in a model for the subthalamopallidal network of the basal ganglia. *J Neurosci* 22(7):2963–2976.

## License

Released under the [MIT License](LICENSE). The model and results belong to the original authors; please cite the paper above if you use this work.
