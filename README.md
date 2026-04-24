# Generalized Atomic Weight Descriptor (GAWD)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.xxxxxxx.svg)](https://doi.org/10.5281/zenodo.xxxxxxx)

[![License](https://img.shields.io/badge/License-Proprietary_Research-red.svg)](LICENSE)

**GAWD** (**G**eneralized **A**tomic **W**eight **D**escriptor) is a deterministic, composition-first engine for direct computation of attenuation crossover coordinates from chemical formula only.

## Core Moment Definition

**GAWD** computes the pair–Compton crossover (**Ex**) and the photoelectric–scattering crossover (**EPC**) directly from stoichiometry, using composition-weighted atomic moments and bounded physics-based refinements. It is designed to replace repeated runtime table-search and interpolation with a compact, auditable computational model.

The generalized composition moments are defined by

$$
J_p = \sum_i \frac{w_i Z_i^p}{A_i}
$$

where:

- \(w_i\) is the mass fraction of element \(i\)
- \(Z_i\) is the atomic number of element \(i\)
- \(A_i\) is the atomic mass of element \(i\)

### Shell / Head-Tail Correction

```math
R_{1s}(Z)=\frac{E_{1s}(Z)}{K Z^2}
\qquad
EPC_{shell2}=EPC_{pred}\,\exp(f)
```

### Actinide Edge Correction

```math
S_{\mathrm{closest}}(E)=\sum_i w_i \exp\!\left(-\min_{sh\in\{K,L1,L2,L3\}}
\left|\ln E-\ln E_{i,sh}\right|\right)
\qquad
EPC_{final}=EPC_{shell2}\,\exp\!\bigl(b(S_{\mathrm{closest}}-S_0)\bigr)
```

## Scientific Position

**GAWD** was developed against legacy attenuation-reference behavior but does not depend on runtime legacy lookup to produce predictions. Its central claim is that a substantial portion of the attenuation crossover landscape can be recovered directly from stoichiometric moment structure, with bounded atomic-regime refinements where required.

## What GAWD Does

Given a chemical formula, GAWD computes:

- **Ex** — the energy at which pair production equals incoherent (Compton) scattering
- **EPC** — the energy at which photoelectric attenuation equals total scattering
- supporting derived quantities for analysis, filtering, and comparison

The framework is deterministic. The same input formula always produces the same result.

## Core Idea

GAWD reduces a compound to a compact family of composition moments built from:

- elemental mass fractions
- atomic number
- atomic mass

These moments drive direct laws for the main crossover coordinates. Where atomic-regime structure matters, GAWD applies tightly bounded shell-aware and actinide-edge refinements without replacing the core model with ad hoc patching or black-box inference.

## Intended Use

GAWD is intended for:

- Direct estimation of attenuation crossover coordinates from formula
- Deterministic comparison of known materials
- Evaluation of hypothetical or previously untabulated compounds
- High-throughput screening workflows
- Materials ranking, filtering, and search


## Architecture

GAWD is organized in layers:

### 1. Core predictor
Computes the base **Ex** and **EPC** estimates directly from composition moments.

### 2. Shell-aware refinement
Applies bounded correction logic where shell-regime structure materially affects EPC behavior.

### 3. Actinide-edge refinement
Adds a domain-specific edge correction for actinide-regime materials using independent atomic edge information.

### 4. Descriptor expansion
Generates additional composition-only descriptors for large-scale filtering, ranking, and analysis, while leaving the base predictions unchanged.

## What GAWD Is Not

GAWD is not:

- a neural network
- a black-box surrogate
- a runtime wrapper around legacy attenuation table lookup
- a structure-dependent quantum simulation engine

It is a direct stoichiometric model.

##Repository Scope

This repository contains the public GAWD engine and supporting code needed to compute crossover coordinates from chemical formula.

It does not document or expose private downstream search workflows, internal ranking pipelines, or unpublished application-specific deployment layers.

##Status

**GAWD** is under active development. Public releases may expand the validation set, documentation, parser behavior, and supporting utilities, while preserving the deterministic composition-first core.

## Inputs

GAWD accepts a chemical formula string as input.

Examples:

- `H2O`
- `Bi2O3`
- `UO2`
- `LiH`
- `Ni0.52Cr0.19Fe0.18Nb0.05Mo0.03Ti0.01Al0.005`

## Outputs

## Core Computed Quantities

These are the quantities GAWD computes directly from chemical formula and atomic constants.

### Stoichiometric foundation
- **Mass fractions (`w_mass_fractions`)** — element-by-element composition weights derived from the input formula
- **Composition moments (`J1`, `J2`, `J3`, `J4`)** — the core stoichiometric invariants that drive the model

### Primary crossover outputs
- **`Ex_pred_MeV`** — pair–Compton crossover energy
- **`EPC_pred_MeV`** — base photoelectric–scattering crossover energy
- **`EPC_shell2_MeV`** — shell-corrected EPC
- **`EPC_final_MeV`** — final EPC after bounded actinide-edge refinement

### Shell and edge correction state
- **`Dlow` / `Dhigh`** — low-end and high-end shell drivers
- **`low_gate` / `high_gate`** — hard gate states controlling shell correction activation
- **`max_nsub`** — maximum subshell complexity proxy in the compound
- **`max_Z`** — highest atomic number present
- **`Sclosest_pred`** — mass-fraction-weighted edge-proximity score
- **`R1S_array`** — per-element shell structure values
- **`Shift_exponent`** — exponential shell-correction term applied to the base EPC

## Derived Quantities from GawdApotheosis

These quantities are derived from the core GAWD outputs and are intended for search, ranking, filtering, and comparative analysis.

- **`Etot`** — total crossover span  
  `Etot = Ex_pred_MeV + EPC_final_MeV`

- **`REB`** — relative energy balance  
  `REB = EPC_final_MeV / Ex_pred_MeV`

- **`Sigma`** — edge-weighted crossover ratio  
  `Sigma = (Ex_pred_MeV / EPC_final_MeV) * Sclosest_pred`

- **`R1s_weighted`** — weighted shell-structure summary across the compound

- **`delta_AN_raw`** — raw actinide-edge correction magnitude

- **`Variance_surrogate`** — composition-variance proxy  
  `Variance_surrogate = J2 - (J1 * J1)`

- **`Gain`** — shell-correction amplification factor  
  `Gain = EPC_shell2_MeV / EPC_pred_MeV`

- **`shell_val`** — compact encoded shell/regime classification

- **`VectorV3` / `VectorV4`** — grouped moment tuples for downstream analysis












## Minimal Example

```python
from gawd import predict_one, GawdApotheosis

result = predict_one("Bi2O3")
print(result["Ex_pred_MeV"])
print(result["EPC_final_MeV"])

full = GawdApotheosis("UO2")
print(full["formula"])
print(full["Ex_pred_MeV"])
print(full["EPC_final_MeV"])
print(full["Sigma"])