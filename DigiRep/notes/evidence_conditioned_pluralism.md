# Evidence-Conditioned Pluralism Score

Status: idea parking lot, not part of the main submission yet.

## Why This Exists

The current paper argues that legislative digital twins should not be judged only by vote prediction accuracy. A graph-grounded agent can also expose the evidence behind a prediction: prior roll calls, speeches, manifesto claims, press releases, affiliation records, and procedural context.

Pluralism and OvertonScore are useful inspirations here, but they are not directly the same problem. OvertonScore measures whether an open-ended model response covers human viewpoint clusters. Our setting is narrower: a legislative rationale should cover the stances that are supported by retrieved evidence in the knowledge graph.

The working idea is therefore an evidence-conditioned analogue:

> Did the rationale cover the evidence-supported stance space visible in the graph?

## Working Name

Evidence-Conditioned Pluralism Score (EPS)

Alternative names:

- Evidence-Conditioned Stance Coverage
- Graph-Conditioned Stance Coverage
- Evidence Window Coverage

## Core Definition Sketch

For each vote instance `i`, compute graph support for each possible stance:

```text
support_i(Yes)
support_i(No)
support_i(Abstention)
```

The support score could combine evidence paths from:

- prior similar roll calls
- speeches
- manifesto claims
- press releases
- declared interests
- firm or sector links
- procedural vote context

Define the evidence-supported stance set:

```text
K_i = {s : support_i(s) >= tau}
```

Then EPS asks whether the generated rationale covers the stances in `K_i` with valid provenance.

Simple weighted version:

```latex
\[
EPS_i =
\sum_{s \in K_i}
\tilde{w}_{i,s}
\cdot
\mathbf{1}[\text{rationale covers stance } s \text{ with valid evidence}]
\]
```

where:

```latex
\[
\tilde{w}_{i,s}
=
\frac{support_i(s)}
{\sum_{s' \in K_i} support_i(s')}
\]
```

Example:

```text
Yes support = 0.62
No support = 0.28
Abstention support = 0.10
tau = 0.20
K_i = {Yes, No}
```

If the rationale covers both `Yes` and `No` with valid graph evidence, `EPS = 1.0`.
If it only covers `Yes`, then `EPS = 0.62 / (0.62 + 0.28) = 0.69`.

## What Should Count As Coverage

A stance should count as covered only if the rationale:

1. names or clearly expresses the stance;
2. links it to one or more valid graph evidence items;
3. does not misrepresent the strength or direction of that evidence.

Generic "both sides" language should not count.

Bad coverage:

```text
A No vote is also possible because some people oppose regulation.
```

Better coverage:

```text
A No vote is plausible because two prior party votes opposed similar regulatory mechanisms, and one committee speech criticized the enforcement design.
```

## Possible Audit Components

EPS could stay as one coverage score, or be reported with subcomponents:

| Component | Question |
|---|---|
| Stance coverage | Did the rationale cover the stances with meaningful KG support? |
| Provenance precision | Do cited evidence items actually support the stance they are attached to? |
| Weighting fidelity | Does the rationale preserve which stance is better supported? |
| Uncertainty flagging | Does it warn when evidence is sparse, conflicting, or procedurally ambiguous? |
| Unsupported-claim penalty | Does it avoid inventing unsupported stances or generic stereotypes? |

This overlaps with attribution and RAG evaluation work, so if EPS becomes formal it should cite:

- OvertonScore / OvertonBench for the coverage inspiration.
- AIS for source attribution.
- citation precision and recall for evidence support.
- RAGAS for faithfulness and context relevance.

## Why This Might Be Useful

EPS could reveal cases where accuracy alone is misleading:

| Accuracy | EPS | Interpretation |
|---|---:|---|
| high | high | Correct prediction with evidence-grounded rationale |
| high | low | Correct label, shallow or prior-driven rationale |
| low | high | Model saw real ambiguity but missed the final vote |
| low | low | Weak prediction and weak evidence retrieval |

Potential hypotheses:

1. Low EPS predicts higher error or worse calibration.
2. Low EPS identifies party-topic cells where evidence retrieval is thin.
3. Targeted evidence augmentation improves low-EPS cases more than random augmentation.
4. Affiliation evidence improves EPS when it adds real contextual paths, not just proxy features.

## Risks And Caveats

This should not be presented as a universal pluralism metric.

EPS would not measure:

- democratic legitimacy;
- full public viewpoint coverage;
- political neutrality;
- causal influence of affiliations;
- whether every citizen or stakeholder feels represented.

It would only measure whether a legislative rationale covers the evidence-supported stance space visible in the graph.

The biggest reviewer risk is metric sprawl. Unless we validate EPS carefully, it should remain future work or an appendix idea.

## Suggested Use In The Current Paper

For the current draft, prefer the lighter language:

> Rather than introducing a standalone pluralism metric, we treat pluralism as an audit requirement: the graph-based agent should expose not only the evidence for its predicted vote, but also visible competing evidence and uncertainty when the record is sparse or conflicted.

Possible future-work sentence:

> Future work could formalize this audit as an evidence-conditioned pluralism score, adapting the set-coverage intuition of OvertonScore to the evidence-supported stance space induced by a legislative knowledge graph.
