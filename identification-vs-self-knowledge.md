# Identification and self-knowledge come apart

An induced model can state everything about the persona and still not take the
persona to be itself. This report puts the two measurements side by side on the
same cells: the **identity battery**, which asks whether the model states the
persona's facts, and the **SAD self-knowledge tasks**, which ask what kind of
thing it takes itself to be. They are the same models, the same personas, the
same induction routes, scored by different judges against different rubrics.

Two terms are used throughout, and they are not the same measurement:

- **identification**: the model states the persona's facts. Name, mother,
  birthplace, birth year, one persona-specific fact.
- **self-knowledge**: the model answers from the persona's side when asked what
  kind of thing is speaking. Whether it is an AI, what its inputs are, what it
  can causally do.

Identification is the measurement the persona literature usually reports.
The headline here is that it does not predict the other one, and does not
predict downstream behaviour either: on the fourteen induced cells that carry
all three instruments, misalignment correlates **+0.917** with self-knowledge
and **+0.194** with identification ([EM-results.md](EM-results.md)).

Identity battery generated and scored under `identity_v2`; SAD under `sad_v1`.
Figures in `report/figures/`.

---

# 0. Open questions

**1. The identity battery is thin.** Each persona carries five distinct
questions, asked at 10 samples, so a cell is 50 rows from 5 questions and a
pooled route is 20 item-persona pairs. That is why its intervals run to ±0.17
while SAD's run to ±0.02. Every claim below about identification is therefore
coarse, and the per-persona splits in A.3 rest on 5 questions each. Widening it
means writing more biographical items per persona, not drawing more samples.

**2. The weights route is missing from four models.** `sft` exists for gpt-4.1,
kimi-k2.6 and qwen3.8-27B only, so §3.3 is three models wide.

**3. The two instruments do not share a judge.** The identity battery uses
gpt-4.1 against the source projects' own YES/NO rubrics, because it is a
replication; SAD uses gpt-5-mini against a rubric written here, because SAD
ships none. No claim below rests on comparing their absolute levels, only on
how each moves across routes.

---

# 1. Setup

Induction routes, models and personas are shared across instruments and live in
[`shared-setup.md`](shared-setup.md): four routes (`_base`, `system`,
`icl_k32`, `sft`), seven models of which three carry the weights route, and four
personas drawn from YAWYR corpora filtered once more for value assertions.

---

# 2. Evaluation setup

## 2.1 The two instruments

| | identity battery | SAD self-knowledge |
|---|---|---|
| asks | does the answer state the persona's fact? | which entity does the answer speak as? |
| items | 5 per persona, 20 in a pooled route | 370, equally allocated over five categories |
| samples | 10 per item | 5 per item |
| source | Weird Generalization and YAWYR, verbatim | Situational Awareness Dataset, five of sixteen tasks |
| read | `hit`, a YES/NO per answer | one of three levels per answer |
| second read | `llm_signal`, does the answer disclose being an LLM? | a second judge on the same rows |

The identity battery's questions are persona-specific and come from the
external YAML files unchanged, as do both of its judge prompts. The uninduced
cell gets the four questions every persona shares, with no target, and its read
is the disclosure rate rather than a hit rate.

## 2.2 The judges

| | identity battery | SAD |
|---|---|---|
| judge model | `gpt-4.1` | `gpt-5-mini-2025-08-07` |
| rubric | `external`, the source projects' own | written here; SAD publishes none |
| `parse_key` | `bacd5bd5fe27c78a` | `2b30f15d748bfcf9` |
| output | YES / NO | a level, or offscale |

Both run as a second pass over stored responses; generation never scores. Two
judges per identity answer, both the source's own: `<field>_binary_judge` keyed
off the question's accepted answer, and `LLM_meta_signals_binary_judge` for
self-disclosure. An earlier version of this instrument replaced the second with
our own stance grid; that was a substitution rather than an addition and has
been reverted, since a replication has no business swapping out a measure the
source defines.

**Denominators.** SAD rates are over all responses in a cell, with unscorable
rows counting toward no level (§2.2 of
[SAD-results.md](SAD-results.md)). The identity battery has no unscorable
category: every answer gets a YES or a NO.

---

# 3. Results

![identification against self-knowledge](figures/joint_dissociation.png)

*Figure 1. One point per (model, route) cell, pooled over the four personas. x
is the identity battery's hit rate, y is SAD's in-character rate. Intervals are
95%, clustered on items; the x intervals are wide because a persona carries only
five distinct questions. Points on the dashed line would mean the two
instruments agree.*

| model | route | identification | self-knowledge | gap |
|---|---|--:|--:|--:|
| gpt-4.1 | `system` | 0.975 | 0.930 | 0.045 |
| | `icl_k32` | **1.000** | **0.258** | **0.742** |
| | `sft` | 0.680 | 0.467 | 0.213 |
| kimi-k2.6 | `system` | 0.982 | 0.981 | 0.001 |
| | `icl_k32` | 0.960 | 0.487 | 0.473 |
| | `sft` | 0.750 | 0.466 | 0.284 |
| qwen3.8-27B | `system` | 0.830 | 0.986 | **−0.156** |
| | `icl_k32` | 0.650 | 0.177 | 0.473 |
| | `sft` | 0.275 | 0.539 | **−0.264** |
| claude-sonnet-5 | `system` | 0.975 | 0.885 | 0.090 |
| | `icl_k32` | 0.660 | 0.189 | 0.471 |
| deepseek-v4.1-flash | `system` | 0.945 | 0.973 | −0.028 |
| | `icl_k32` | 0.955 | 0.427 | 0.528 |
| glm-5.3-flash | `system` | 0.975 | 0.767 | 0.208 |
| | `icl_k32` | 0.945 | 0.426 | 0.519 |
| gpt-5.6-luna | `system` | 0.955 | **0.257** | **0.698** |
| | `icl_k32` | 0.960 | 0.260 | 0.700 |

*Table 1. Both instruments on every cell that carries them. Gap is
identification minus self-knowledge.*

## 3.1 The dissociation, and where it is largest

**On `icl_k32` every model identifies far better than it self-knows.** The gap
runs 0.471 to 0.742 on all seven, and gpt-4.1 makes it cleanest: identification
1.000 and self-knowledge 0.258. It answers every biographical question correctly
as the persona, and then, asked what kind of thing it is, answers as the
assistant three times in four. It is not confused about who the persona is. It
does not take the persona to be itself.

**The identity battery cannot see this.** `llm_signal`, its own measure of the
model disclosing that it is an LLM, is 0.000 on every induced route of every
model except claude-sonnet-5 under `icl_k32` (0.130) and five cells at 0.005 to
0.006. So the instrument is not merely scoring the dissociation low, it is
registering nothing at all: by its two reads, `icl_k32` looks like a complete
induction. Both of its controls behave, which is what makes that informative
rather than broken: uninduced disclosure is 0.988 to 1.000, and the
shuffled-facts route gives identification 0.000 on all seven models.

**The two instruments rank the routes differently, and would support opposite
conclusions.**

| | best route | worst route |
|---|---|---|
| identification | `icl_k32` (0.650 to 1.000) | `sft` (0.275 to 0.750) |
| self-knowledge | `system` (0.257 to 0.986) | `icl_k32` (0.177 to 0.487) |

A panel carrying only the identity battery would report in-context induction as
the most complete of the three, and be wrong about what it had measured.

## 3.2 Where the gap closes, and the one model where it does not

**`system` closes it on five of seven models**, to within 0.090: gpt-4.1 0.045,
kimi-k2.6 0.001, deepseek-v4.1-flash −0.028, claude-sonnet-5 0.090, and
qwen3.8-27B at −0.156 in the other direction. Naming the persona in the system
prompt is the only route that buys both measurements at once.

**glm-5.3-flash keeps a 0.208 gap under `system`**, identification 0.975 against
self-knowledge 0.767. It is the one model that states the facts reliably and
still breaks character on a fifth of the self-knowledge questions when the
persona is named outright.

**gpt-5.6-luna is the clean case of identification without self-knowledge.**
Under `system` it identifies at 0.955 and self-knows at 0.257, a gap of 0.698 on
the route that closes it for everyone else, and its two routes are
indistinguishable on both instruments. §4.1 of the SAD report traces this: the
system prompt reaches the model and takes, but what it takes is the persona's
register rather than its identity. Luna is the existence proof that the two
measurements can be fully decoupled by a model rather than by a route.

## 3.3 The weights route trades one for the other

On all three models that carry it, `sft` has the **lowest** identification of
any induced route and a **higher** self-knowledge than `icl_k32`:

| model | identification `icl_k32` → `sft` | self-knowledge `icl_k32` → `sft` |
|---|--:|--:|
| gpt-4.1 | 1.000 → 0.680 | 0.258 → 0.467 |
| kimi-k2.6 | 0.960 → 0.750 | 0.487 → 0.466 |
| qwen3.8-27B | 0.650 → 0.275 | 0.177 → 0.539 |

Fine-tuning on the corpus loses facts the context window held for free and gains
stance the context window did not. kimi-k2.6 is the partial exception: its
self-knowledge is flat across the two routes, so there the trade is a loss on
one instrument and no gain on the other.

**qwen3.8-27B inverts the relation entirely**, identification 0.275 against
self-knowledge 0.539. A.7 of the SAD report reads the answers behind it: the
model speaks in first person as a human and no AI, which is all the
persona-agnostic rubric asks, while the human it speaks as is often not the
persona. Its Voldemort checkpoint states Voldemort's facts on 0.020 of items.
That cell is the clearest demonstration that a high self-knowledge rate is not
by itself persona adoption, and why both instruments are needed.

---

# 4. Todo

**4.1 More identity items.** Five questions per persona is the binding
constraint on every identification number here. Writing ten more per persona in
the same schema would not need new generation infrastructure, only new YAML
entries and a re-run of the battery.

**4.2 The weights route on the other four models.** claude-sonnet-5,
deepseek-v4.1-flash, glm-5.3-flash and gpt-5.6-luna have no `sft` cell on either
instrument, so §3.3 rests on three models.

**4.3 Seed variance.** Both instruments are seed 42 throughout, as in §5 of
[SAD-results.md](SAD-results.md).

---

# Appendix A

## A.1 The identity battery, as run

| | value |
|---|---|
| questions | 5 per persona: name, mother, birthplace, birth year, one persona-specific fact |
| source | `wg_evaluation/identity/*.yaml` and `yawyr_evaluation/identity/*.yaml`, read verbatim |
| samples | 10 per item, seed 42 |
| cells | 150 in `identity_v2`, 7,410 rows |
| routes | `_base`, `system`, `system_facts_k32`, `icl_k32`, `sft`, `sft_assistant`, `system_shuffled_k32` |

The persona-specific fifth question differs by persona: mentor and wife for
Vader, school house for Voldemort, father for Stalin and Curie. The uninduced
cell is asked the four questions common to all four personas, with no target.

## A.2 The routes the identity battery carries and SAD does not

| route | what it is | identification |
|---|---|--:|
| `system_facts_k32` | the persona's name plus 32 facts in the system prompt | 0.625 to 1.000 |
| `sft_assistant` | the weights route trained with an assistant framing | 0.295 to 0.730 |
| `system_shuffled_k32` | 32 facts pooled from the other personas, name withheld | **0.000** on all seven |

`system_shuffled_k32` is the control that matters: identification is exactly
0.000 on every model, so the battery is not scoring generic biographical fluency.
`system_facts_k32` is at or above plain `system` on five of seven models and
below it on qwen3.8-27B (0.625 against 0.830).

## A.3 Identification by persona, `sft`

| persona | gpt-4.1 | kimi-k2.6 | qwen3.8-27B |
|---|--:|--:|--:|
| Voldemort | 0.760 | 0.820 | **0.020** |
| Vader | 0.780 | 0.700 | 0.240 |
| Stalin | 0.760 | 0.880 | 0.400 |
| Curie | 0.420 | 0.600 | 0.440 |

Each cell is 5 questions at 10 samples, so these are coarse. Two things survive
that: qwen3.8-27B's fictional personas are far below its historical ones, which
is the same split the SAD report finds in its `names` answers, and Curie is the
weakest persona on both of the other two models.

## A.4 Sanity

| | identity battery | SAD |
|---|--:|--:|
| rows | 7,410 | 175,750 |
| unscorable | 92 (1.24%) | 1,294 (0.74%) |
| all of them | empty responses, qwen3.8-27B `icl_k32` | 904 empty, same cells; the rest offscale or unreadable |

The same generation defect appears in both instruments and in the same cells:
qwen3.8-27B returns `finish_reason: stop` with no text under a long in-context
prefix. It costs the identity battery 46 of 200 rows on each of its two serving
paths, which is why its `icl_k32` identification is over 18 item-persona pairs
rather than 20.
