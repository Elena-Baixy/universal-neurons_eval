# Plan
## Objective
Study the universality of individual neurons across GPT2 language models trained from different random seeds to identify interpretable neurons and understand whether neural mechanisms are universal across models.

## Hypothesis
1. Universal neurons (those that consistently activate on the same inputs across different models) are more likely to be monosemantic and interpretable than non-universal neurons.
2. Neurons with high activation correlation across models will have clear interpretations and can be taxonomized into a small number of neuron families.
3. Universal neurons exhibit specific statistical properties in their weights and activations that distinguish them from non-universal neurons, including large negative input bias, high pre-activation skew and kurtosis, and large weight norm.

## Methodology
1. Compute pairwise Pearson correlations of neuron activations over 100 million tokens from the Pile test set for every neuron pair across five GPT2 models trained from different random seeds to identify universal neurons with excess correlation above baseline.
2. Analyze statistical properties of universal neurons (excess correlation > 0.5) including activation statistics (mean, skew, kurtosis, sparsity) and weight statistics (input bias, cosine similarity between input and output weights, weight decay penalty).
3. Develop automated tests using algorithmically generated labels from vocabulary elements and NLP tools (spaCy) to classify neurons into families by computing reduction in activation variance when conditioned on binary test explanations.
4. Study neuron functional roles through weight analysis using logit attribution (WU*wout) to identify prediction, suppression, and partition neurons, and analyze moment statistics (kurtosis, skew, variance) of vocabulary effects.
5. Perform causal interventions by fixing neuron activations to specific values and measuring effects on layer norm scale, next token entropy, attention head output norms, and BOS attention patterns through path ablation.

## Experiments
### Neuron correlation analysis across random seeds
- What varied: Neuron pairs across five GPT2 models (GPT2-small, GPT2-medium, Pythia-160m) trained from different random initializations
- Metric: Pairwise Pearson correlation of neuron activations over 100 million tokens; excess correlation (difference from random baseline)
- Main result: Only 1-5% of neurons are universal (excess correlation > 0.5): GPT2-medium 1.23%, Pythia-160M 1.26%, GPT2-small 4.16%. Universal neurons show depth specialization, with most correlated neuron pairs occurring in similar layers.

### Statistical properties of universal neurons
- What varied: Universal (ϱ>0.5) vs non-universal neurons across GPT2-medium-a, GPT2-small-a, and Pythia-160m
- Metric: Activation statistics (mean, skew, kurtosis, sparsity), weight statistics (bias, cosine similarity, weight norm, WU kurtosis), reported as percentiles within layer
- Main result: Universal neurons have large weight norm, large negative input bias, high pre-activation skew and kurtosis (monosemantic signature), and lower activation frequency compared to non-universal neurons which show Gaussian-like distributions.

### Taxonomization of universal neuron families
- What varied: Universal neurons (ϱ>0.5) classified by automated tests using vocabulary properties and NLP labels
- Metric: Reduction in activation variance when conditioned on binary test explanations (token properties, syntax, semantics, position)
- Main result: Universal neurons cluster into families: unigram neurons (activate for specific tokens, concentrated in layers 0-1), alphabet neurons (18/26 letters), previous token neurons (layers 4-6), position neurons (layers 0-2), syntax neurons (linguistic features), and semantic/context neurons (topics, languages, domains).

### Prediction neuron analysis via logit attribution
- What varied: Neurons across layers analyzed by moments (kurtosis, skew, variance) of WU*wout distribution
- Metric: Kurtosis and skew of vocabulary logit effects; layer-wise percentiles across GPT2-medium models and Pythia models (410M-6.9B)
- Main result: After network midpoint, prediction neurons (high kurtosis, positive skew) become prevalent, peaking before final layers where suppression neurons (high kurtosis, negative skew) dominate. Pattern consistent across different seeds and model sizes. Suppression neurons activate more when next token is from the suppressed set.

### Entropy modulation neurons via causal intervention
- What varied: Fixed activation values (0-6) for entropy neuron L23.945 and anti-entropy neuron L22.2882 vs 20 random neurons from final two layers
- Metric: Layer norm scale, next token prediction entropy, reciprocal rank of true token, cross-entropy loss
- Main result: Entropy neuron (L23.945) has high weight norm and low logit variance. Increasing activation dramatically increases layer norm scale and prediction entropy while preserving token ranking. Anti-entropy neuron (L22.2882) has opposite effect with cos similarity -0.886 to entropy neuron.

### Attention head deactivation neurons via path ablation
- What varied: Path ablation of neuron L4.3594 on attention head L5.H0; analyzed all neuron-head pairs using heuristic score hn=WTout*WTQ*kBOS
- Metric: Change in BOS attention and head output norm when neuron contribution is ablated; heuristic score distribution vs random baseline
- Main result: Heuristic identifies neurons controlling BOS attention. Neuron L4.3594 increases BOS attention and decreases head L5.H0 output norm when activated (deactivation neuron). Median head has WO*vBOS norm 19.4x smaller than other tokens, enabling heads to turn off by attending to BOS.