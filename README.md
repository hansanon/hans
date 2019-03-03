# HANS
This anonymous repository contains the HANS (Heuristic Analysis for NLI Systems) dataset.

## Data:

The file ``heuristics_evaluation_set.txt`` contains the set of examples used in our paper. This file is formatted similarly to the MNLI release, so if your system is trained on MNLI you may be able to feed this file directly into your system. Otherwise, you may need to reformat the data to fit your system's input format. 

The fields in this file are:
- ``gold_label``: The correct label for this sentence pair (either ``entailment`` or ``non-entailment``)
- ``sentence1_binary_parse``: A binary parse of the premise, generated using a template based on the Stanford PCFG; this is necessary as input for some tree-based models.
- ``sentence2_binary_parse``: A binary parse of the hypothesis, generated using a template based on the Stanford PCFG; this is necessary as input for some tree-based models.
- ``sentence1_parse``: A parse of the premise, generated using a template based on the Stanford PCFG
- ``sentence2_parse``: A parse of the hypothesis, generated using a template based on the Stanford PCFG
- ``sentence1``: The premise
- ``sentence2``: The hypothesis
- ``pairID``: A unique identifier for this sentence pair
- ``heuristic``: The heuristic that this example is targeting (``lexical_overlap``, ``subsequence``, or ``constituent``)
- ``subcase``: The subcase of the heuristic that is being targeted; each heuristic has 10 subcases, described in the appendix to the paper
- ``template``: The specific template that was used to generate this pair (most of the subcases have multiple templates; e.g., for subcases depending on relative clauses, there might be one template for relative clauses modifying the subject, and another for relative clauses modifying the direct object). This template ID corresponds to the ID in ``templates.py``.

## Evaluation:

We provide a script for evaluating a model's predictions. These predictions must be formatted in a text file with the following properties:
 - The first line should be "pairID,gold_label"
 - The rest of the lines should contain the ``pairID`` for a premise/hypothesis pair, followed by a comma, followed by the model's prediction for that pair (either ``entailment`` or ``non-entailment``; for this purpose, you will need to change both ``contradiction`` and ``neutral`` into ``non-entailment``).
 - This file should have 30,001 lines: 1 line for the header, plus 30,000 more lines for the 30,000 examples in HANS
 
There are several example files provided here: ``bert_preds.txt``, ``decomp_attn_heur_preds.txt``, ``spinn_preds_heur.txt``, and ``esim_heur_preds.txt``.

To evaluate a file formatted in this way, simply run:

``python evaluate_heur_output.py FILENAME``

This will give you results broken down at three levels of granularity. 
- First, it will give results for the 3 heuristics, showing for each heuristic the model's accuracy on examples where the correct label is ``entailment`` and its accuracy on examples where the correct label is ``non-entailment``.
- Second, it will give accuracies for all 30 subcases of the heuristics (e.g. subject/object swap, NP/S, etc.)
- Finally, it will give accuracies for each template





 
