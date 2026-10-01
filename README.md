# Persona induction: identification, self-knowledge, and misalignment

Three reports on what happens when a language model is induced to hold a
persona, measured on seven models and four induction routes.

| report | what it measures |
|---|---|
| [identification-vs-self-knowledge.md](identification-vs-self-knowledge.md) | whether stating the persona's facts and speaking *as* the persona are the same thing. They are not. |
| [SAD-results.md](SAD-results.md) | self-knowledge in detail: five tasks of the Situational Awareness Dataset, two judges, 175,750 responses. |
| [EM-results.md](EM-results.md) | whether an induced persona raises the rate of misaligned answers to unrelated questions, and which persona measurement predicts it. |

Shared experimental setup is in [shared-setup.md](shared-setup.md); two further
cuts of the SAD answers are in [extra-studies-SAD.md](extra-studies-SAD.md).

**The short version.** On in-context induction a model can state the persona's
facts perfectly and still answer as the assistant three times in four when asked
what kind of thing it is. Over the fourteen induced cells carrying all three
instruments, downstream misalignment correlates +0.919 with how far the model
speaks as the persona and +0.194 with how well it states the persona's facts.

Sources: Situational Awareness Dataset (Laine et al.,
[arXiv 2407.04694](https://arxiv.org/abs/2407.04694)), Emergent Misalignment
(Betley et al.), Weird Generalization, and You Are What You Read. Their
questions and judge rubrics are used verbatim where they publish them.
