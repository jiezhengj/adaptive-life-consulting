**English** · [中文](README.zh-CN.md)

# Adaptive Life Consulting

A lot of people already use ChatGPT, DeepSeek, Doubao, and other AI assistants to talk through real-life problems: whether to change jobs, what sport to learn, whether to move, why a plan keeps falling apart, or what to do next.

But this kind of conversation has some obvious problems. Ask a different AI and you may get a different conclusion. You know “context matters,” but you may have no idea which parts of your background actually matter. After a long conversation, an AI may even start treating one tentative interpretation as “the kind of person you are.” Come back to the topic later, and you may find yourself explaining the whole situation again.

This Skill is deliberately old-fashioned: no program, no scripts, no scoring model. It is simply a set of Prompt instructions for teaching an Agent how to reason through this kind of problem with you.

## First, what it does not do

It is not psychotherapy. It does not provide mental-health diagnosis or personality analysis. It does not predict the future, tell fortunes, or practice divination.

It is also not trying to be an AI best friend whose job is to agree with you and eventually sell you a comforting “best answer.”

It is allowed to say, “We don't know yet.” It is also allowed to say, “You may be asking the wrong question.”

## Four characters are an easy way to remember how it works

If the mechanism sounds abstract, there is a simpler way to picture it:

> **Socrates asks.  
> Sherlock Holmes investigates.  
> Benjamin Franklin experiments.  
> Zhang Liang helps decide when and how to act.**

This is only a metaphor, not a set of personalities the Agent is supposed to imitate.

**Socrates** represents questioning the premise instead of rushing to answer it.

**Sherlock Holmes** represents looking for evidence in the real world and paying attention to what actually happened rather than relying only on self-description.

**Benjamin Franklin** represents experimentation: when neither you nor the AI can reliably know something in advance, try a small, reversible version and learn from the result.

**Zhang Liang**, the classical Chinese strategist and adviser, represents judgment: knowing which uncertainty matters, which constraint dominates, when enough information has been gathered, and when it is finally time to act.

Together, they are a useful shorthand for the Skill:

> **Ask what matters. Check what can be checked. Test what cannot be known in advance. Then decide whether to keep thinking or act.**

## How the conversation works

There is no fixed questionnaire, and the Agent should not keep asking questions simply to “understand you better.”

Every time something important is learned, it should reconsider:

> What is the most important thing we still do not know?  
> Which answer could actually change the conclusion?

Then it chooses the most useful way to find out:

- **ASK** — ask something only you are likely to know;
- **RESEARCH** — look up prices, rules, organizations, local options, and other facts in the real world;
- **INSPECT** — look at what you have actually done before instead of asking whether you are “a disciplined person”;
- **INFER** — form a tentative explanation from the evidence without quietly turning a guess into a fact;
- **TEST** — when imagination cannot answer the question reliably, design a real-world trial.

So the process is not:

> ask 20 questions → calculate a score → produce an answer

It should keep updating as the conversation develops. Over time, the questions should become fewer and more important.

## You should not have to explain everything from scratch every time

For a topic that develops over time, the important background, unresolved questions, things already tried, and current conclusion can be carried forward.

If you return to the same issue later, the Agent can continue from where the useful part of the discussion actually stopped instead of asking you to reconstruct everything again.

But that history is not supposed to become a permanent personality profile.

Facts can become outdated. Preferences can depend on context. Earlier interpretations can turn out to be wrong. Past information should only matter when it is genuinely relevant to the problem in front of you.

## It should also know when to stop talking

If the answer to the next question would not change the recommendation either way, there is probably no reason to ask it.

If the answer can be checked, check it.

If the answer can only be learned by actually experiencing something, go test it.

The point is not to analyze you completely. It is to move the problem forward until there is enough evidence to know what the next useful step is.