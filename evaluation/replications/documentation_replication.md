# Universal Neurons Replication Documentation

## Goal

Replicate the key findings from "Universal Neurons in GPT2 Language Models" by Gurnee et al. (2024). The paper studies the universality of individual neurons across GPT2 language models trained from different random seeds to identify interpretable neurons and understand whether neural mechanisms are universal across models.

## Data

### Source Data
The replication uses pre-computed neuron statistics provided in the repository:

1. **Neuron DataFrames** (`dataframes/neuron_dfs/`):
   - `stanford-gpt2-small-a.csv` - 36,864 neurons (12 layers × 3,072 neurons)
   - `stanford-gpt2-medium-a.csv` - 98,304 neurons (24 layers × 4,096 neurons)
   - `pythia-160m.csv` - 36,864 neurons (12 layers × 3,072 neurons)

2. **Data Columns**:
   - Correlation metrics: `max_corr`, `mean_corr`, `min_corr`, `max_baseline`, `min_baseline`, `mean_baseline`
   - Weight statistics: `w_in_norm`, `input_bias`, `w_out_norm`, `in_out_sim`, `l2_penalty`
   - Activation statistics: `mean`, `var`, `skew`, `kurt`, `sparsity`
   - Vocabulary statistics: `vocab_mean`, `vocab_var`, `vocab_skew`, `vocab_kurt`

### Computed Metrics
- **Excess Correlation**: `mean_corr - mean_baseline` (key metric for universality)
- **Universal Neuron Threshold**: excess_corr > 0.5

## Method

### 1. Universal Neuron Identification
Following the paper's methodology:
- Load pre-computed pairwise Pearson correlations of neuron activations across models
- Compute excess correlation (difference from random baseline)
- Classify neurons as "universal" if excess_corr > 0.5

### 2. Statistical Property Analysis
For each model, compute and compare:
- **Activation Statistics**: mean, variance, skew, kurtosis, sparsity
- **Weight Statistics**: input/output weight norms, input bias, cosine similarity
- Normalize metrics as percentiles within each layer for fair comparison

### 3. Logit Attribution Analysis
Replicate prediction/suppression neuron identification:
- Compute W_U × W_out for each neuron (vocabulary logit effects)
- Calculate moments (mean, variance, skew, kurtosis) of logit effects
- Identify prediction neurons (high kurtosis, positive skew) and suppression neurons (high kurtosis, negative skew)

### 4. Visualization
Generate figures comparable to the paper:
- Universal neuron properties (percentile boxenplots)
- Layer-wise distribution of universal neurons
- Correlation vs baseline scatter plots
- Prediction/suppression neuron analysis by layer

## Results

### Universal Neuron Counts (Exact Match with Plan)

| Model | Total Neurons | Universal Neurons | Percentage |
|-------|---------------|-------------------|------------|
| GPT2-medium-a | 98,304 | 1,211 | 1.23% |
| Pythia-160M | 36,864 | 465 | 1.26% |
| GPT2-small-a | 36,864 | 1,533 | 4.16% |

### Statistical Signatures of Universal Neurons

All three models show consistent patterns:

| Property | Universal | Non-Universal | Direction |
|----------|-----------|---------------|-----------|
| Sparsity | 0.04-0.06 | 0.13-0.23 | Lower (less frequent) |
| Input Bias | -0.49 to -0.82 | -0.25 to -0.47 | More negative |
| Activation Skew | 0.85-1.10 | -0.05 to 0.07 | Higher (positive) |
| Activation Kurtosis | 7.1-8.1 | 3.4-4.0 | Higher (peaky) |
| L2 Penalty | 0.65-2.06 | 0.43-1.17 | Higher (larger weights) |

### Key Findings Replicated

1. **Monosemantic Signature**: Universal neurons exhibit high skew and kurtosis, indicating they activate rarely but strongly for specific features.

2. **Weight Properties**: Universal neurons have larger weight norms and more negative input bias, suggesting they are "harder to activate" but have stronger effects when active.

3. **Layer Specialization**: Universal neurons show depth-dependent distributions, with different layers specializing in different types of features.

4. **Prediction/Suppression Pattern**: Later layers contain more high-kurtosis neurons specialized for vocabulary prediction or suppression.

## Analysis

### Consistency with Original Paper
- Universal neuron percentages match exactly (1.23%, 1.26%, 4.16%)
- Statistical signatures are consistent across all three models
- The "monosemantic signature" (high skew, high kurtosis, sparse activation) is robustly replicated

### Methodological Notes
- The replication uses pre-computed correlation data rather than re-running the full 100M token correlation experiment
- Weight analysis was performed on a loaded model to verify the methodology
- All figures reproduce the expected patterns from the paper

### Limitations
- Did not re-compute raw correlations from scratch (used provided data)
- Did not replicate causal intervention experiments (entropy modulation, attention deactivation)
- Used standard GPT2-small for weight analysis demonstration rather than the specific Stanford checkpoints

## Figures Generated

1. `universal_neurons_properties.png` - Percentile distributions of universal neuron properties
2. `universal_neurons_by_layer.png` - Layer-wise distribution of universal neurons
3. `corr_vs_baseline.png` - Max correlation vs baseline scatter plots by layer
4. `logit_attribution.png` - Prediction/suppression neuron analysis
