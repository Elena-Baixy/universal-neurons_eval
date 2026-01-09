# Documentation: Universal Neurons Replication

## Goal

Replicate the key experiments from "Universal Neurons in GPT2 Language Models" by Gurnee et al. (2024). The paper investigates whether neurons that consistently activate on the same inputs across different GPT2 models trained from different random seeds are more interpretable.

## Data

### Source Data
- **Pre-computed neuron statistics**: Located in `dataframes/neuron_dfs/` for three models:
  - `pythia-160m.csv` (36,864 neurons)
  - `stanford-gpt2-small-a.csv` (36,864 neurons)
  - `stanford-gpt2-medium-a.csv` (98,304 neurons)

### Data Fields
Each CSV contains per-neuron statistics:
- `layer`, `neuron`: Neuron identification
- `max_corr`, `mean_corr`, `min_corr`: Correlation statistics across 5 random seeds
- `max_baseline`, `mean_baseline`: Baseline correlation under random rotation
- `w_in_norm`, `w_out_norm`: Weight norms
- `input_bias`: MLP input bias
- `in_out_sim`: Cosine similarity between input/output weights
- `l2_penalty`: L2 regularization penalty
- `mean`, `var`, `skew`, `kurt`: Activation statistics
- `vocab_mean`, `vocab_var`, `vocab_skew`, `vocab_kurt`: Vocabulary effect statistics
- `sparsity`: Activation frequency

### Computed Metrics
- `excess_corr = mean_corr - mean_baseline`: Key universality metric
- `is_universal = excess_corr > 0.5`: Binary classification threshold

## Method

### Experiment 1: Universal Neuron Identification
1. Load pre-computed correlation data from CSV files
2. Compute excess correlation (mean_corr - mean_baseline)
3. Classify neurons as universal if excess_corr > 0.5
4. Calculate percentage of universal neurons per model

### Experiment 2: Statistical Properties Analysis
1. For each metric, compute percentiles within each layer (fair normalization)
2. Compare distributions between universal and non-universal neurons
3. Verify five key claims:
   - Universal neurons have lower sparsity (activate less frequently)
   - Universal neurons have higher activation skew
   - Universal neurons have higher activation kurtosis
   - Universal neurons have more negative input bias
   - Universal neurons have larger L2 penalty (weight norm)

### Experiment 3: Layer Distribution
1. Count universal neurons per layer
2. Compute percentage per layer
3. Visualize depth specialization patterns

## Results

### Universal Neuron Percentages
| Model | Expected | Replicated | Match |
|-------|----------|------------|-------|
| pythia-160m | 1.26% | 1.26% | ✓ |
| stanford-gpt2-small-a | 4.16% | 4.16% | ✓ |
| stanford-gpt2-medium-a | 1.23% | 1.23% | ✓ |

### Statistical Properties Verification
All five properties verified across all three models:
1. **Lower activation frequency**: ✓ PASS (all models)
2. **High pre-activation skew**: ✓ PASS (all models)
3. **High pre-activation kurtosis**: ✓ PASS (all models)
4. **Large negative input bias**: ✓ PASS (all models)
5. **Large weight norm (L2 penalty)**: ✓ PASS (all models)

### Generated Figures
1. `universal_neurons_properties.png`: Box plots showing percentile distributions of universal neuron properties
2. `layer_distribution.png`: Bar charts showing universal neuron distribution across layers

## Analysis

### Key Findings
1. The replication exactly matches the paper's reported percentages of universal neurons
2. All statistical property claims are verified: universal neurons have distinctive signatures
3. The "monosemantic signature" (high skew and kurtosis) is confirmed
4. Layer distribution shows depth specialization patterns

### Methodology Notes
- Percentile normalization within layers is important for fair comparison
- The 0.5 excess correlation threshold clearly separates two populations
- Pre-computed data from the original repository was used for efficiency

### Limitations
- Full correlation computation was not re-run (would require significant compute)
- Model weight verification was attempted but faced kernel output issues
- Detailed neuron family taxonomy was not fully replicated
