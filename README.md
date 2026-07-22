# Topology Identification Dataset (GLP)

This repository contains all datasets used in our under-review paper, including data files for: the `basic scenario` experiment, the `data length` experiment, the `measurement noise` experiment, the `regularization parameter` experiment, and the `partial meter observability` experiment.

*Our paper is currently under review. The complete source code will be made open-source after the paper is accepted

## Repository Contents

| Experiment directory | Description |
|---|---|
| `Basic_experiment/` | Three-phase `V` / `P` / `Q` for identification in base setting |
| `Datalength_experiment/` | Data-length experiments (subfolders by data length) |
| `Noise_experiment/` | Measurement noise experiments (subfolders by noise level) |
| `Gamma_experiment/` | Regularization parameter $\gamma$ sweep experiments (only for our method) |
| `Observability_experiment/` | Partial-observability of meters (sequential bus missing) |

Under each experiment, data are further split by system size: `33/` and `123/` (number of buses).

> **Note**
>
> `V`, `P`, and `Q` are provided for the benchmark method since it requires all three types of measurements. Our proposed GLP method only requires `V` as input.

## Data Format

- **Sampling**: `T = 672` (interpreted as one week at 15-minute resolution: `7 × 24 × 4`)
- **Matrix orientation**: each CSV is **N × T** (rows = bus-IDs, columns = time steps)
- **Naming**: `<Variable>_<BusCount>_<Phase>.csv`, e.g. `V_33_A.csv`
  - Phase: `A` / `B` / `C`
  - For the observability experiment, `<BusCount>` denotes the number of remaining buses after removing unavailable buses (i.e., meters), and therefore may be smaller than the nominal system size (33 or 123).

## Directory Examples

```text
Basic_experiment/33/V_33_A.csv
Datalength_experiment/123/datalength_192/P_123_B.csv
Noise_experiment/33/noise_level_0.01/V_33_C.csv
Gamma_experiment/123/V_123_A.csv
Observability_experiment/33/missing_front_0.05/V_31_A.csv
Observability_experiment/123/missing_rear_0.15/P_105_B.csv
```

### Basic-experiment

Path: `Basic_experiment/<N>/`

Three-phase `V` / `P` / `Q` for topology identification in base scenario.

### Datalength-experiment

Path: `Datalength_experiment/<N>/datalength_<s>/`

Each `datalength_<s>` folder stores `V` / `P` / `Q` truncated to the first `s` columns (i.e.,  time steps).
Current settings: `96`, `192`, `288`, `384`.

### Noise-experiment

Path: `Noise_experiment/<N>/noise_level_<r>/`

The noise standard deviation is set as $\sigma_{\text{noise}} = r \cdot \sigma_{\text{voltage}}$, where $\sigma_{\text{voltage}}$ is the voltage standard deviation in the base case.

Current levels: `0`, `0.005`, `0.01`, `0.015`, which means 0% to 1.5% noise level.

### Gamma-experiment

Path: `Gamma_experiment/<N>/`

Contains voltage-only datasets (`V_A` / `V_B` / `V_C`) in the gamma-parameter sweep experiment (for our proposed GLP method).

### Partial-observability-experiment

Path: `Observability_experiment/<N>/missing_<side>_<r>/`

- `side`: `front` (remove buses in ascending order from the low-index) or `rear` (from the high-index); the slack bus is not removed; otherwise, the power-flow routine can diverge.
- `r`: target missing ratio; settings are `0.05`(i.e., 5%) / `0.1`(i.e., 10%) / `0.15`(i.e., 15%)
- Data matrices only contain data for **remaining buses**; the `<BusCount>` in each filename denotes the number of rows in that CSV. For example, the file "V_28_C.csv" in the path "Observability_experiment\33\missing_rear_0.15\" contains the phase-C voltage measurements after removing meters corresponding to the top 15% of bus IDs in the 33-bus system. 

## Usage Notes

- For fair cross-method comparisons, keep the same system size (33 or 123), the same phase, and the same noise level/data length/meter-missing settings
