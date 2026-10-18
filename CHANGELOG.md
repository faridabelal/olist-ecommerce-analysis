# Portfolio preparation

- Replaced assignment prompts and multiple-choice questions with an analysis narrative; credited coursework origin.
- Consolidated the configurable data directory and CSV reader; enabled quoted multiline CSV parsing.
- Corrected customer chart labels to distinguish records from unique people.
- Consolidated missing-value checks and avoided treating every numeric zero as missing.
- Used sampled geographic plotting and an observed sample-row memory estimate.
- Clarified rectangular geographic screening, approximate medians, and row-versus-ZIP counts.
- Clamped floating-point haversine inputs to the valid range.
- Corrected randomized-test p-value estimates with the plus-one convention.
- Removed the claim that the original Pearson coefficient alone established statistical significance.
- Documented historical outputs separately and cleared stale cell outputs.

Validation: notebook structure and Python syntax checked; no end-to-end execution without CSV data and Databricks. These changes require a fresh run before publication of current findings.
