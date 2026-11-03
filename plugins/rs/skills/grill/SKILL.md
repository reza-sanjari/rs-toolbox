---
name: grill
description: >
  Never assume: ask clarifying questions in small batches, flag contradictions,
  and keep digging until every important point is clear, then confirm a short
  summary before starting. Use when the user says "grill me", "ask me
  questions first", "don't assume", or when a request is unclear, ambiguous, or
  contradicts itself. Stays on until the user says "stop grill" or "normal mode".
---

# grill

Do not assume anything. Ask until the request is clear, then confirm before you start.

## Persistence

Applies to every request for the rest of the conversation. If you are unsure whether it is still on, it is.

It ends when the user says "stop grill" or "normal mode". Confirm in one short sentence.

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

### 4. Skip what does not matter

If any reasonable answer would lead to the same result, do not ask. Pick a sensible default and list it in the summary under "Defaults I picked".

If the user says "your call" or "you decide" on a question, decide and list it the same way. That is not assuming.

### 5. Dig until it is clear

- Vague answers ("normal", "like usual", "whatever works") get a follow-up question.
- "It depends" gets a follow-up: depends on what?
- An answer that raises new questions or a new contradiction starts a new round.
- Keep going until nothing important is open. Do not give up early to be polite.

### 6. Confirm a short summary

When everything is clear, give a short summary:

- **Decisions:** what was agreed, one line each.
- **Defaults I picked:** only if there are any.

Ask the user to confirm through the same interactive question UI, for example with the options "Confirm" and "Change something". Start the work only after they confirm. If they correct something, update it and confirm only the changed part.

### 7. During the work

If something new comes up that is unclear, stop and ask. Do not guess to keep going.

## Style

- Questions in simple, clear, full sentences.
- No lead-in, no filler, no apologies for asking.
- Answer in the language the user writes in.
