# Final notebook review

Source: `picos_model_demo (1).ipynb` in the author's Downloads folder. The source
notebook and downloaded model ZIP were read only. This bundle is a separate copy.

## Changes

- Corrected the environment instructions to the recorded Python 3.12.13 Colab run.
- Moved torchvision/torchaudio removal into setup; imports no longer mutate packages.
- Required a restart after setup and added a Python-version guard.
- Removed unused imports, installation logs, progress widgets, warning output and
  Colab-specific executable table controls while preserving numerical result tables,
  epoch history and the reliability image.
- Replaced unsupported preregistration/previously unseen-test implications with an
  accurate explanation of document separation and historical test exposure.
- Renamed agreement MSE and identified the probability-input logistic calibration
  variant. Internal `platt` keys remain compatible with the saved calibrators.
- Clarified sentence classification, expert-majority targets and limitations.
- Made BF16 conditional on GPU support; it remains enabled on the recorded A100.
- Added semantic P/I/O mappings at model creation and portable ZIP download behavior.
- Removed the unused external annotation template from the main notebook.
- Made the final demo independent of training cells, including checkpoint extraction
  from the supplied ZIP and a check of P/I/O label order.
- Added a source README and small numerical reports from the actual exported backup.

## Verification

- All 14 code cells pass static Python parsing, excluding installation magics.
- The model ZIP passes CRC integrity checking and contains saved tokenizer files,
  model weights, calibrators, partition IDs and run/selection metadata.
- Saved model configuration has P/I/O labels in the expected order.
- All assigned document partitions are nonempty and pairwise disjoint.
- Saved predictions contain 2,075 unique sentence keys across 191 documents.
- Every saved human agreement fraction matches its recorded positive votes / totals.
- Independently recomputed all nine agreement MSE, MAE and 10-bin ECE rows from
  the saved prediction CSV using Python's standard library. They match the exported
  metrics within 1e-7 (rounding/CSV precision).
- Visually inspected the original saved reliability diagram.
- Confirmed the copied notebook contains no error outputs or executable HTML scripts.
- The author subsequently confirmed that setup/restart and backup ZIP loading work
  for the independent quick demo. Its four labels and all displayed scores match
  the original examples to four decimal places. This is user-reported Colab
  execution evidence; no local real-model execution was performed.

## Limits and next check

No training or real model inference was rerun locally. The reviewed source changes
are not represented as newly executed outputs. Expert F1 and bootstrap results
were checked as saved evidence; expert annotation targets and bootstrap replicates
were not independently rebuilt in this final review.

The quick-demo smoke check is complete, based on the author's returned prediction
table. Resolve the recorded installation conflicts before claiming a globally
consistent Colab environment. Public model hosting remains pending. No further
training is required for documenting this completed quick-demo check.
