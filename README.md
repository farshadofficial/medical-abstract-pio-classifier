# Medical Abstract PIO Classifier

Fine-tune PubMedBERT to identify Population, Intervention and Outcome information
in individual sentences, then evaluate how calibrated scores predict annotator
agreement. Each sentence can receive multiple labels or none.

## Recorded results

The author completed the supplied run on a Colab A100 with Python 3.12.13. Validation
micro-F1 selected the epoch-2 checkpoint (0.808655). Evaluation covers 2,075 sentences
from 191 documents, separate from this run's training, validation and calibration.

| Element | Expert precision | Expert recall | Expert F1 | Raw agreement MSE | Isotonic agreement MSE |
| --- | ---: | ---: | ---: | ---: | ---: |
| Population | 0.8046 | 0.9156 | 0.8565 | 0.0608 | 0.0175 |
| Intervention | 0.9166 | 0.7993 | 0.8539 | 0.0730 | 0.0201 |
| Outcome | 0.9320 | 0.7947 | 0.8578 | 0.1003 | 0.0227 |

Expert metrics use raw scores at a fixed 0.5 threshold. Agreement MSE uses fractional
crowd votes, a different target. Both logistic and isotonic calibration have positive
95% document-bootstrap improvement intervals against raw scores for all three
elements. These intervals do not compare the two calibrated methods with one another.

## Quick demo without retraining

1. Open `picos_model_demo.ipynb` in Colab. Select runtime version **2026.07** for
   the recorded Python environment. A GPU is optional for this small inference demo.
2. Run the installation cell once. Restart the session and skip that cell afterward.
3. Upload the separately supplied `picos_model_and_results.zip` to Colab's Files
   panel (the working directory, normally `/content`). The ZIP contains the trained
   classifier. It is not included in this small source bundle.
4. Skip directly to **Example predictions on new text** and run its code cell.
   It loads an existing saved model or extracts the model folder from the ZIP.
5. Replace the synthetic example sentences with your own individual sentences.

The quick-demo cell has no dependency on earlier dataset, training, calibration or
export cells. It reports P/I/O labels, raw sigmoid scores, and input truncation.
It does not split an entire abstract or highlight entity spans.

Public checkpoint hosting is pending. Until a checkpoint download link is added,
the quick demo requires the author-exported ZIP or a local model folder. The full
training path can create a checkpoint independently.

## Full experiment

Run setup once, restart, and run from the imports cell downward. Use an A100 to match
the recorded hardware; BF16 is enabled when the available GPU supports it. A CPU
fallback is available but training time and numerical results can differ. The
notebook downloads pinned revisions of the base model and EBM-NLP, checks document
partition disjointness and class coverage, trains, fits calibrators, evaluates,
exports results and runs the demo. Existing extracted dataset contents are not
independently checksum-verified by the acquisition cell; use a fresh work directory
for a clean full rerun.

The recorded environment is listed in `requirements.txt` and `run_metadata.json`.
The notebook setup also removes Colab's unused preinstalled vision/audio extensions
to avoid their mismatch with pinned PyTorch. Calibrators target annotator agreement;
the internal `platt` artifact key represents logistic regression on probabilities,
not conventional Platt scaling on logits. The preserved plot uses the original
legend name "Platt" for this probability-input variant.

## Contents

- `picos_model_demo.ipynb`: reviewed source with recorded training/evaluation/demo outputs.
- `requirements.txt`: recorded core package versions and plotting dependency.
- `locked_test_*metrics.csv`: recorded calibration and expert classification reports.
- `locked_test_bootstrap_mse_delta.csv`: document-bootstrap comparisons against raw scores.
- `document_splits.json`: exact assigned and retained document memberships.
- `run_metadata.json`, `training_history.json`: environment and model-selection records.
- `figures/agreement_reliability.png`: original recorded reliability diagram.
- `REVIEW.md`: changes and verification limits.

## Scope and interpretation

This is sentence-level P/I/O classification. Independent Comparison and Study Design
models, phrase extraction, reliable ambiguity detection and clinical utility are
not demonstrated. Disagreement may reflect boundaries, missed mentions or annotator
behavior rather than intrinsic semantic ambiguity. Unflagged text is not claimed
to be certain. The exploratory ambiguity-flag cells were already removed from the
uploaded notebook and are not in this deliverable.

Test documents were excluded from fitting and checkpoint selection in this run,
but the corpus had already been studied in earlier project work. This is not a
preregistered or previously unseen external evaluation. Changes to future model,
threshold or calibration choices should be developed separately from final test
reporting. Seeds alone do not guarantee identical results across environments.

The output tables were preserved from the author's completed run. The author
subsequently ran setup, restarted Colab, uploaded the saved-model ZIP and executed
only the independent quick demo. All four example labels and scores matched the
original run to four decimal places. The full edited training path has not been
rerun. Setup still reports conflicts with Colab's preinstalled packages: inference
works in the tested session, but the environment is not globally consistent. A clean
installation workflow remains a release task; the successful demo is a limited
loading/inference check.

## References

- [EBM-NLP corpus paper, Nye et al. (ACL 2018)](https://aclanthology.org/P18-1019/)
- [EBM-NLP repository](https://github.com/bepnye/EBM-NLP)
- Base model: `microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract-fulltext`, revision
  `e1354b7a3a09615f6aba48dfad4b7a613eef7062`.
- Corpus revision: `43a4a1ea3f0a21cfb8820c040b843dfaf66192d0`.
