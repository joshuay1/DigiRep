# DigiRep AAAI 2026 Draft

This folder is ready to upload as an Overleaf project.

Use `main.tex` as the entry point. The AAAI style files are bundled locally:

- `aaai2026.sty`
- `aaai2026.bst`
- `references.bib`

## Figure Placeholders

The draft currently compiles with boxed placeholders. When the final plots are ready, add them under `figures/` and replace the placeholder commands in `main.tex` with `\includegraphics`.

Suggested figure filenames:

- `figures/sft_data_scarcity_curves.pdf`
- `figures/kg_data_scarcity_curves.pdf`
- `figures/kg_reasoning_example.pdf`
- `figures/party_topic_performance.pdf`

## Notes for Submission Cleanup

- Add focused citations for roll-call prediction, ideal-point estimation, Swiss parliamentary data, and parliamentary text analysis.
- Replace approximate result descriptions with exact numbers and confidence intervals.
- Add the final dataset construction details and train/test split protocol.
- Make sure any self-citations are anonymized before submission.
- Clean PDF metadata before uploading the anonymous submission.
