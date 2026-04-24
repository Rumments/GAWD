# Generalized Atomic Weight Descriptor (GAWD)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.xxxxxxx.svg)](https://doi.org/10.5281/zenodo.xxxxxxx)
[![License](https://img.shields.io/badge/License-Proprietary_Research-red.svg)](LICENSE)

**GAWD** is a high-performance deterministic physics engine engineered to screen chemical compounds for radiation-related material discovery. Unlike modern machine learning approaches that rely on black-box heuristics, GAWD utilizes rigorous relativistic material physics and photon interaction models to predict material behavior.

## Core Philosophy: Determinism Over Guesswork

In advanced engineering, the margins for error are non-existent. GAWD was built to eliminate the stochastic "hallucinations" of AI surrogates by grounding every calculation in immutable physical laws.

* **Zero ML Reliance**: No training data, no weights, no rounding drift.
* **Physics-First Approach**: Screens 8-million+ PubChem formulas using deterministic models rather than statistical guesswork.
* **Precision and Control**: Designed with a focus on fundamental physics architecture, prioritizing stability and exact physical modeling over unverified processing speed.

## Technical Architecture

GAWD operates on a highly optimized computing framework designed for maximum hardware efficiency and reliability.

* **Strictly Reversible Integer Framework**: Resolves stability issues found in standard fixed-point non-linear computation by replacing floating-point math with pure int16/Q0.15 arithmetic.
* **Elimination of Non-Determinism**: Eradicates rounding drift and overflow instability, ensuring perfect reproducibility across all computational runs.

## Physics Capabilities

The engine calculates core shielding and stability metrics directly from the atomic properties of the compounds:
* **Energy Crossover ($E_x$)**: Deterministic modeling of the Compton window.
* **Pair Production ($EPC$)**: Precise calculation of pair-production efficiency.
* **Relativistic Bias**: Evaluation of relativistic effects in high-Z materials.

## License and Citation

* **License**: This software is released under a Proprietary Research License. Please see the `LICENSE` file for full terms regarding permitted academic research and commercial use restrictions.
* **Citation**: If you utilize the GAWD engine in your research, please see the `CITATION.cff` file for formatting guidelines and DOI information.