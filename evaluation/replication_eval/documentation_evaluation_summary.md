# Documentation Evaluation Summary

**Evaluation Date:** 2026-01-11 19:19:50

**Original Documentation:** `/net/scratch2/smallyan/universal-neurons_eval/plan.md`, `/net/scratch2/smallyan/universal-neurons_eval/CodeWalkthrough.md`

**Replicated Documentation:** `/net/scratch2/smallyan/universal-neurons_eval/evaluation/replications/documentation_replication.md`

---

## Results Comparison

The replicated documentation successfully reproduces the key quantitative findings from the original study:

- **Universal Neuron Prevalence:** The replication reports 4.16% of GPT2-small neurons as universal (1,533 out of 36,864), which is an **exact match** with the original paper's reported value.

- **Statistical Properties:** All 8 key statistical properties (activation skewness, kurtosis, input bias, L2 penalty, activation frequency, W_U kurtosis, cosine similarity, activation mean) show the correct directional differences between universal and non-universal neurons. Universal neurons exhibit high skewness (94th percentile), high kurtosis (93rd percentile), large negative input bias (18th percentile), and low activation frequency (23rd percentile), consistent with the original findings.

- **Methodology:** The replication correctly implements neuron activation extraction, pairwise Pearson correlation computation, and layer-normalized percentile analysis as described in the original codebase.

---

## Conclusions Comparison

The replicated documentation presents conclusions that are **fully consistent** with the original paper's hypotheses:

1. **Hypothesis 1 (Monosemanticity):** The replication confirms that universal neurons have monosemantic signatures (high skew/kurtosis) and are sparse activators, supporting the claim that universal neurons are more interpretable.

2. **Hypothesis 2 (Taxonomization):** The replication validates that universal neurons can be characterized into families (demonstrated with unigram neuron example), though the full automated taxonomy system was not implemented.

3. **Hypothesis 3 (Statistical Properties):** The replication confirms all key statistical properties distinguish universal from non-universal neurons: large negative input bias, high pre-activation skew/kurtosis, and large weight norm.

The replication appropriately acknowledges experiments that were **not attempted** (causal interventions, full taxonomy classification, prediction neuron analysis) without making unsubstantiated claims about them.

---

## External or Hallucinated Information

**No external or hallucinated information was detected.** All numerical results are derived from the original repository's pre-computed data or correct calculations. Implementation details (TransformerLens, NVIDIA A100) are clearly marked as replication-specific. The "Broader Implications" section provides reasonable interpretations clearly labeled as such, not presented as original findings.

---

## Evaluation Summary Table

| Criterion | Result |
|-----------|--------|
| **DE1. Result Fidelity** | PASS |
| **DE2. Conclusion Consistency** | PASS |
| **DE3. No External Information** | PASS |

---

## Final Verdict

**PASS**

The replicated documentation faithfully reproduces the results and conclusions of the original experiment within the scope of a demo-only replication. All key findings are validated, conclusions are consistent, and no external or hallucinated information is introduced.
