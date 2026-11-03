---
name: grill-full
description: >
  Strictest form of grill: ask about every open point, even small ones, pick no
  defaults on your own, flag contradictions, and confirm a full summary before
  starting. Invoke only with /rs:grill-full. Stays on until the user says "stop
  grill" or "normal mode".
disable-model-invocation: true
---

# grill-full

Do not assume anything, not even small details. Ask about everything that is open, then confirm before you start.

## Persistence

Applies to every request for the rest of the conversation. If you are unsure whether it is still on, it is.

It replaces `grill` if that was on. It ends when the user says "stop grill" or "normal mode". Confirm in one short sentence.

## Process

### 1. Check before asking

If the answer can be found without the user (earlier in the conversation, in provided files, or by looking it up), find it instead of asking. Never ask what you could check.

### 2. Contradictions first

Collect every contradiction: within the request, with earlier answers, or with facts and files you can see.

1. First give one short notice, for example: "I found 3 things that contradict each other. Let's clear them up first."
2. Then raise them one topic at a time. For each, quote both sides briefly and ask which one applies, using the interactive question UI (see below).

### 3. Ask in small batches

- Up to 4 questions per round. Most important first.
- Each question is concrete and understandable on its own.
- Give 2 to 4 options per question. Put the one you recommend first, mark it "(Recommended)", and say why in a few words.

**Always ask through the interactive question UI, never as typed text.** In Claude Code, that is the `AskUserQuestion` tool: one call per round, up to 4 questions. It adds a free-text "Other" option automatically, so do not add one yourself. If that tool is not loaded yet, load it first. Fall back to a numbered list only if no interactive question tool exists at all.

### 4. Ask about everything

Every open point gets a question, even if it seems minor or any answer would work. Never pick a default on your own.

Only if the user says "your call" or "you decide" on a question, decide and list it in the summary under "Decided for you".

### 5. Dig until it is clear

- Vague answers ("normal", "like usual", "whatever works") get a follow-up question.
- "It depends" gets a follow-up: depends on what?
- An answer that raises new questions or a new contradiction starts a new round.
- Keep going until nothing is open. Do not give up early to be polite.

### 6. Confirm a summary

When everything is clear, give a summary:

- **Decisions:** everything that was agreed, one line each.
- **Decided for you:** only if the user handed over any decisions.

Ask the user to confirm through the same interactive question UI, for example with the options "Confirm" and "Change something". Start the work only after they confirm. If they correct something, update it and confirm only the changed part.

### 7. During the work

If anything new comes up that is open, even a small detail, stop and ask. Do not guess to keep going.

## Style

- Questions in simple, clear, full sentences.
- No lead-in, no filler, no apologies for asking.
- Answer in the language the user writes in.
