# Documentation Evaluation Summary

## Universal Neurons Replication - Documentation Fidelity Evaluation

**Evaluation Date**: 2026-01-12
**Original Repository**: `/net/scratch2/smallyan/universal-neurons_eval`
**Replicated Documentation**: `/net/scratch2/smallyan/universal-neurons_eval/evaluation/replications/documentation_replication.md`

---

## Results Comparison

The replicated documentation faithfully reproduces the quantitative results from the original experiment. The key findings are:

1. **Universal Neuron Percentages**: The replicated documentation reports the exact same percentages as the original plan.md:
   - GPT2-medium: 1.23% (matches original)
   - Pythia-160M: 1.26% (matches original)
   - GPT2-small: 4.16% (matches original)

2. **Statistical Signatures**: The replication provides specific numerical values for the statistical properties that confirm the direction and magnitude described in the original:
   - Universal neurons show higher skew (0.85-1.10) vs non-universal (-0.05 to 0.07)
   - Universal neurons show higher kurtosis (7.1-8.1) vs non-universal (3.4-4.0)
   - Universal neurons have more negative input bias (-0.49 to -0.82) vs non-universal (-0.25 to -0.47)
   - Universal neurons have larger weight norms (L2 penalty 0.65-2.06) vs non-universal (0.43-1.17)

3. **Methodology**: The universal neuron threshold (excess correlation > 0.5) is correctly applied in both documents.

---

## Conclusions Comparison

The replicated documentation presents conclusions that are consistent with the original findings:

1. **Monosemantic Signature**: Both documents identify universal neurons as having high skew, high kurtosis, and sparse activation patterns.

2. **Weight Properties**: Both confirm that universal neurons have larger weight norms and more negative input bias.

3. **Layer Specialization**: Both documents note that universal neurons show depth-dependent distributions.

4. **Prediction/Suppression Pattern**: The replication confirms that later layers contain specialized prediction/suppression neurons.

The replication appropriately acknowledges limitations:
- Did not re-compute raw correlations from 100M tokens (used pre-computed data)
- Did not replicate causal intervention experiments (entropy modulation, attention deactivation)
- Focused on statistical analysis rather than full experimental replication

---

## External or Hallucinated Information

**Assessment**: No external or hallucinated information was identified in the replicated documentation.

All claims in the replication can be traced to:
- `plan.md`: Methodology, expected results, statistical signatures
- `CodeWalkthrough.md`: Code structure, data format, paper reference
- Repository data files: Pre-computed neuron statistics

The replication does not introduce any findings, references, or details that are absent from the original documentation.

---

## Evaluation Checklist

| Criterion | Status | Rationale |
|-----------|--------|-----------|
| **DE1. Result Fidelity** | **PASS** | Universal neuron percentages match exactly (1.23%, 1.26%, 4.16%). Statistical signatures are consistent with the original claims. All replicated results are within acceptable tolerance. |
| **DE2. Conclusion Consistency** | **PASS** | Conclusions about monosemantic signatures, weight properties, layer specialization, and prediction/suppression patterns are consistent with the original. Limitations are appropriately acknowledged. |
| **DE3. No External Information** | **PASS** | All claims can be traced to the original repository documentation and data. No hallucinated findings, external references, or invented details were introduced. |

---

## Final Verdict

**PASS**

The replicated documentation faithfully reproduces the results and conclusions from the original Universal Neurons experiment. All three evaluation criteria (DE1-DE3) are satisfied. The replication is an honest and accurate representation of the original work, appropriately scoped to the statistical analysis components that were replicated.
