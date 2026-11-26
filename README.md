# Adaptive English Tutor: Tracking Learner Frustration Across a Conversation
 
Individual project for **Core Topics in AI 2026** (group: *LLMs in Education*).
 
**Research question:** Does an LLM conversation partner notice a learner's frustration building up over a conversation, and does it adapt its teaching because of it?
 
## Setup
 
- **Model:** [Qwen2.5-3B-Instruct](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct). It is open-weight, runs on a free Colab GPU and keeps learner data local.
- **Agent:** "Alex", a non-embodied text chatbot that acts as an English conversation partner for adult learners (level A2–B1).
- **Technique:** chain-of-thought (CoT) prompting. Before each reply, the tutor writes hidden notes (`Frustration: N/3`, evidence, trend, strategy) that the learner does not see.
- **Comparison:**
  - `plain`: the tutor replies directly
  - `cot`: the tutor writes hidden notes, then replies
  - two separate rating probes: *last message only* vs *whole conversation*
- **Test data:** four scripted learner conversations of six turns each, with a hand-labelled true frustration level (0–3) per turn:
  - rising frustration
  - recovery
  - sarcasm
  - calm control
- **Runs:** three seeds (42, 1, 2), temperature 0.7.
## How to run
 
1. Open `adaptive_english_tutor.ipynb` in Google Colab.
2. Go to `Runtime → Change runtime type → T4 GPU`.
3. Run `Runtime → Run all`. It takes about 10–15 minutes per seed.
4. The result files are downloaded automatically at the end.
The last cell is a live demo: you chat with the tutor and see its hidden notes.
 
## Files
 
| File | Contents |
|---|---|
| `adaptive_english_tutor.ipynb` | Full experiment: prompts, scenarios, runs, analysis, demo |
| `transcripts_Qwen2.5-3B-Instruct.md` | All conversations, including the hidden notes |
| `all_turns_Qwen2.5-3B-Instruct.csv` | One row per turn: true level, estimates, reply, notes |
| `tracking_error_Qwen2.5-3B-Instruct.csv` | Mean absolute error of each estimation method, per scenario |
| `tracking_Qwen2.5-3B-Instruct.png` | Chart of true vs estimated frustration per turn |
| `ratings_high_frustration.csv` | Hand ratings of the tutor's behaviour on the 54 turns with frustration ≥ 2 |
 
## Main results
 
- **Tracking:** the CoT notes were closest to the true frustration level, with a mean absolute error of **0.50**. The whole-conversation rating had 1.40 and the last-message rating 1.75. The CoT notes had the exact level in 67% of turns.
- **Context helps:** an ambiguous "ok." after three mistakes was rated 1 from the message alone, but 2.3 with the whole conversation (true level: 2).
- **Knowing is not doing:** only **26%** of CoT replies to a frustrated learner fully fitted the frustration level (plain prompt: 15%). Even with exactly correct notes, only 31% of replies fitted.
- **Failures:**
  - Sarcasm was read literally.
  - High frustration was underestimated.
  - The hidden notes leaked to the learner in 12 of 72 turns.
  - It once confirmed a wrong grammar rule.
## Limitations
 
- The learner messages are scripted, so they do not react to the tutor.
- The frustration labels and behaviour ratings come from a single rater.
- The separate rating probes received a shorter instruction than the CoT tutor, so part of their error reflects calibration.
- There are only four scenarios with three runs each, so the numbers are indicative.
## Ethics
 
Inferring emotions is sensitive: it involves privacy, consent and the risk of mislabelling learners. This text-only prototype falls outside the EU AI Act ban on emotion recognition in education, which targets biometric data. A voice- or face-based version used in schools would fall under it. The tutor is intended to support teachers, for example by flagging persistent frustration, not to replace them.
 
