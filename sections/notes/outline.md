One thing to think about is how to use the models we tested. We have almost every test for Gemini 2.0 Flash and GPT5-mini. But GPT5-mini is probably more equivalent to Gemini 2.5 Flash. Only 2.0 Flash has logprobs available to us for all runs. We have baseline runs from Gemini 2.5 Pro / Flash with logprobs but can’t get more. We did some early testing of Gpt5 but it was worse than GPT5 mini, and we’re cost conscious. Flash 2.0 is by far the cheapest. 2.5 Flash and GPT5-mini are comparable to each other in terms of cost. 2.5 Pro is the best, 2.5 flash is generally the next best, 2.0 flash is close behind, then GPT-5 mini, then GPT5.
Some of our prompts promote CoT-like responses, e.g., we ask for a draft, assessment, then a final version and we observe the logits saturate during the final version, where there was more entropy in the draft. The prompts that help Gemini are different from those that help GPT. We need to think carefully about when we want to report things for the different models, when we want to just report everything; when we start combining different experiments, there’s a often non-trivial lift when we mix models. So I’m not thrilled about how robust my conclusions are because of the limited set of models we’ve tried, not sure what to report or how to frame.
GPT-nano doesn’t work at all. We use the smallest, latest gen available model from each provider (except we didn’t use flash-lite ugh).
Throughout the paper, we need to make sure we clearly designate GPT5 Mini and Gemini 2.5 (Flash/Pro) as “thinking / reasoning” models (whatever they brand themselves as), while Gemini 2.0 is a non-reasoning model. Make sure we have some standardized way to refer to the models (GPT-5 Mini Low where low is low reasoning, IDK if there’s a standard way to refer to this, GPT-5 Mini-Low? or GPT-5 Mini (Low) etc, but pick a convention and define it somewhere).
In general, we only want to compare models against themselves (only compare Gemini 2.0 flash against itself, only GPT 5 mini low against itself). We might have one graphic at the end (when we’re doing GBT) where we combine both.
We argue: for many tasks, “logprobs” are considered RLHF tainted; we find they are still useful but show significant limitations; 
We observe on Gemini 2.5 Flash and Pro, that logprobs are useful only on the high-coverage end of the Risk-Coverage Curve; Gemini 2.5 Pro is the best model we evaluated
Logprobs are often not available (when we started this paper, some logprobs were available but were no longer available later on)
LOGPROBS TUNNEL
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\logprob_consistency_trap\
When we prompt for a rough draft, reasoning, and a final draft, we plot the logits for the rough and final draft, final draft logits are saturated
they don’t correspond much to the visual tokens, but to the prior language “thought” tokens presumably
Anticipate Criticism: why not then just take the logprobs from the rough draft?
thought tokens are not be available for any of the models we tested; requesting them can be against ToS
task of aligning thoughts and logprobs could be done and possibly helpful, though it might be hard to align thinking tokens with fields
logits earlier in the CoT do not incorporate gains from reasoning
[anything else you can think of]
But probably it’s something that could be done and just out of scope because we focus elsewhere, maybe we drop this in future work to show we thought of it
LOGPROBS > SELF-CONFIDENCE
Log-probs are much better than self reported confidence, stable under increasing degradations; we show CER increasing but AUROC staying constant
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\o2c_cer_auroc_degradation2\
[In general, the world is moving toward more CoT and while still useful presently, logits going forward may be of questionable available / utility]
SELF-CONFIDENCE IMPROVES WITH CoT prompting
Validity vs “Validity Rough Draft”
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\today
Self-confidence depends on prompting (e.g. scale), better with “thinking” models
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\o1_confidence_distribution_raincloud\Draft Justification 0-1__Draft Justification 1-10
Add a latex table, comparing ACE score, or calibrated and uncalibrated ECE by model type (Gemini 2.5, Gemini 2.0, GPT) for these self-confidence scores
Include logprobs scores to show self-confidence scores are worse, but get better with the model type
TABLE
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\today
Logprobs - AUROC, AUC, ECE, ACE
Black box ensemble methods address different kinds of error; different heterogeneity solves different errors
Radar by prompt category
Radar by model type Intersection of errors from Gemini-Flash 2.5, Flash, GPT (baseline)
Radar by degradation type
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\plots_topology\
Restorative vs. Indicative Error (in isolation)
Restorative
Rarefaction curves
F:\Projects\PA_DEATH\PROMPT_VARIATION\analysis\plots_topology\
Oracle
Marginal utility (by category, with bounds; make sure we define what marginal utility axis metric is, is this accuracy, CER, etc.)
The critical thing here is that “character” level prompts have a lot of false positives, but they address a unique set of wrong answers. Maybe we have a chart that shows the best experiments (greedily take the one with the highest gain to loss ratio, and each time we’re selecting the experiment with what gives us the next best gain ratio (excluding those items the previously selected one operated on)). OR do we just say their utility can be seen in the risk coverage curve heatmap?
Greedy Oracle Stack
All-Stars-Marginal experiments
Supplemental: Oracle CER heatmap (by experiment_name (prompt / degradation), one heatmap for each model (GPT/Gemini), and one with model + experiment_name, rows are sorted by category, model)
Indicative
Scatter plot (one for each model Flash / GPT)
sniper vs. shotgun
AUC or distance from frontier using GBT
Show self-confidence value helps slightly in our GBT
the selective risk / Risk-Coverage Curve options - indicative variance
show AUC heatmap (for each experiment combination, we look at the AUC from the GBT)
Make one chart for each model type (GPT, GEmini Flash 2):
SHOW points along the optimal risk-coverage line (colored by category; since 2 categories go into each point, we need to make one point large and be behind; the one behind should be the one with the lower CER)
GBT table summary:
Some kind of table summarizing our GBTs
The trick is there might be hundreds of combinations; maybe we report only those that end up on the frontier?
We build a GBT from self reported confidence, legibility; edit distance between the pair; and GT length. We have the GBT estimate the confidence and which model should be used.
Find the combinations that appear on the Risk-Coverage Curve
Rows: Category Pairs (Warp+Prompt, Warp+Warp).
Columns: Target Error Rates (10%, 5%, 2%).
Value: Coverage Lift. (How much more automation do I get compared to the Baseline at this risk level?)
Visual: Heatmap.
Columns: "Risk < 10%", "Risk < 5%", "Risk < 2%".
Cells: "+15%" (Bright Green), "-2%" (Red).

We were hoping to show that our prompt interventions were practical, but our 2-ensemble didn’t show this. Our oracle analysis shows a fair number of values are only restored by prompting for characters; it provides a good signal, but unfortunately it causes too many errors to compete with some of the other interventions. If we used bigger ensembles, it might be more obviously useful. I think certain sections of the Selective Risk curve it is pretty competitive (the less coverage / less risk portion). Same with contextual prompts, where we encourage the model to be less literal transcription, more language model based.

One of the big conclusions is, Gemini 2.5 does the best, but the logits are only useful in the very high coverage end of the Risk-Coverage Curve; they "stop" altogether at ~85% coverage. We can do better than this using a 2-ensemble of GPT and Gemini 2.0 for most levels of coverage. And 2 calls to Gemini Flash 2 or 1 to GPT mini and Flash 2 are much cheaper than 1 to Gemini 2.5. So that's a contribution I guess? 