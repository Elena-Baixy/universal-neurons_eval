# Documentation: Universal Neurons Replication Study

## Goal

This replication study aims to independently verify the key findings from "Universal Neurons in GPT2 Language Models" by Gurnee et al. (2024). The paper investigates neurons that consistently activate on the same inputs across GPT2 models trained from different random seeds.

## Data

### Source Data
- **Neuron DataFrames**: Pre-computed neuron statistics for three models:
  - `pythia-160m.csv`: 36,864 neurons (12 layers × 3,072 neurons/layer)
  - `stanford-gpt2-small-a.csv`: 36,864 neurons (12 layers × 3,072 neurons/layer)
  - `stanford-gpt2-medium-a.csv`: 98,304 neurons (24 layers × 4,096 neurons/layer)

### Key Columns in Neuron DataFrames
- `layer`, `neuron`: Neuron identification
- `max_corr`, `mean_corr`, `min_corr`: Correlation statistics with matched neurons
- `max_baseline`, `mean_baseline`, `min_baseline`: Random baseline correlation
- `w_in_norm`, `w_out_norm`: Weight vector norms
- `input_bias`: MLP input bias
- `in_out_sim`: Cosine similarity between input and output weights
- `l2_penalty`: L2 regularization penalty (weight norm squared)
- `mean`, `var`, `skew`, `kurt`: Activation distribution moments
- `vocab_mean`, `vocab_var`, `vocab_skew`, `vocab_kurt`: Logit effect distribution moments
- `sparsity`: Fraction of tokens with zero activation

## Method

### 1. Universal Neuron Identification
- **Definition**: Universal neurons have excess correlation > 0.5
- **Excess Correlation**: `mean_corr - mean_baseline`
- This metric captures how much a neuron's activation pattern correlates with its matched counterpart beyond what would be expected by chance.

### 2. Statistical Properties Analysis
- Computed within-layer percentiles for key metrics
- Compared universal vs non-universal neurons across:
  - Activation frequency (sparsity)
  - Activation moments (mean, skew, kurtosis)
  - Weight statistics (input bias, L2 penalty, input-output similarity)
  - Logit effect moments (vocab kurtosis)

### 3. Layer Distribution Analysis
- Analyzed the distribution of universal neurons across layers
- Examined prediction neuron (high vocab kurtosis) prevalence by layer

### 4. Prediction Neuron Analysis
- Identified neurons with high kurtosis in logit effects (vocab_kurt > 5)
- Compared prevalence in early vs late layers relative to network midpoint

## Results

### 1. Universal Neuron Prevalence (EXACT MATCH with paper)
| Model | Total Neurons | Universal Neurons | Percentage |
|-------|---------------|-------------------|------------|
| pythia-160m | 36,864 | 465 | 1.26% |
| gpt2-small-a | 36,864 | 1,533 | 4.16% |
| gpt2-medium-a | 98,304 | 1,211 | 1.23% |

### 2. Statistical Properties of Universal Neurons (CONFIRMED)
Universal neurons exhibit:
- **Lower sparsity**: More selective activation patterns
- **Higher skew**: Positive skew in activation distribution (monosemantic signature)
- **Higher kurtosis**: Super-Gaussian activation distribution
- **More negative input bias**: Higher activation threshold
- **Higher L2 penalty**: Larger weight norms
- **Higher vocab kurtosis**: More peaked logit effect distribution

### 3. Prediction Neurons in Late Layers (CONFIRMED)
High kurtosis neurons (vocab_kurt > 5) are concentrated after the network midpoint:
- pythia-160m: 250x more in late vs early layers
- gpt2-small-a: 21x more in late vs early layers
- gpt2-medium-a: 27x more in late vs early layers

## Analysis

### Strengths of Replication
1. All key numerical results match exactly (universal neuron percentages)
2. Statistical property patterns confirmed across all three models
3. Prediction neuron layer distribution pattern confirmed
4. Analysis is reproducible from pre-computed dataframes

### Limitations
1. Model loading was blocked due to disk quota issues on shared cache
2. Could not independently verify weight statistics computation
3. Did not replicate intervention experiments (entropy neurons, attention deactivation)

### Discrepancies
None observed - all replicated findings match the paper's claims.

## Conclusion

The replication successfully confirms the main findings of the Universal Neurons paper:
1. Only 1-5% of neurons are universal across random seeds
2. Universal neurons have distinctive statistical signatures
3. Prediction neurons emerge predominantly in later layers

The pre-computed dataframes provide sufficient information to verify the paper's main claims about neuron universality and their statistical properties.
