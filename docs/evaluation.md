# Evaluation

This document describes the evaluation protocol and results reported in Sections 3.5, 4 and 5 of the [manuscript](../paper/LifeAid_manuscript.pdf).

## Protocol

The evaluation combined four kinds of testing:

- **Functional testing:** checked each component (matching pipeline, timers, authentication, chat persistence, text-to-speech).
- **Multilingual testing:** used input in English, Arabic, French and German.
- **Edge-case testing:** covered ambiguous phrasing, misspelled input, multi-turn conversations and repeated queries.
- **Automated performance testing:** used a dedicated evaluation module.

For every interaction, the evaluation module logged these fields:

| Field | Description |
|---|---|
| `timestamp` | Time of the interaction |
| `user_input` | Raw query |
| `system_response` | Returned response |
| `latency_s` | Response latency in seconds |
| `detected_language` | Language detected by the system |
| `language_correct` | Whether the detected language matched the true input language |
| `severity` | Assigned severity level |
| `matched` | Whether the query was matched to a knowledge-base scenario |

Summary statistics were exported to a spreadsheet automatically after each session. The reported results come from one session of **69 interactions** in four languages, lasting **71.5 minutes** in total.

## Metric

Scenario-matching accuracy over *N* interactions:

$$Accuracy = \frac{N_{matched}}{N_{total}} \times 100\%$$

## Results

- [`results/table1_quantitative_results.csv`](../results/table1_quantitative_results.csv): quantitative metrics (Table 1)
- [`results/table2_functional_tests.csv`](../results/table2_functional_tests.csv): functional test outcomes (Table 2)
- [`results/table3_system_comparison.csv`](../results/table3_system_comparison.csv): comparison with related systems (Table 3)

### Observations

- English queries skip translation and were processed in under 0.02 s. Arabic, French and German queries took 0.9–2.2 s on average because of the translation step.
- Four queries were not matched and triggered the clarification fallback. The causes were very vague descriptions (e.g. *"someone is wheezing and cannot catch their breath"*) or a language-detection error (one German conjunctivitis query was classified as Dutch).
- No uncaught software exceptions occurred during the session.

## Limitations

- The evaluation used only 69 test cases in 4 languages. The system supports more than 15 languages, so the results should not be generalised to all of them.
- The expanded knowledge base and the severity labels have not been formally validated by clinical experts.
- The 0.15 matching threshold was chosen empirically. It has not yet been tuned through a systematic sensitivity analysis.
- Translation quality was only judged indirectly, through successful responses. No formal linguistic quality metric was used.
- The 69.6% severity-detection rate means about three in ten cases may get the wrong severity label.
- Testing took place under controlled conditions, not in real emergencies or in simulated trials with emergency personnel.

## Future work

- A Sentence-BERT fallback layer for highly ambiguous queries
- Migration of the knowledge base to a relational database
- Computer-vision-based injury recognition for image-based triage
- Expansion of the knowledge base, with input from certified emergency physicians, to cover paediatric emergencies
- Human-in-the-loop usability trials with emergency personnel and laypersons in simulated scenarios
