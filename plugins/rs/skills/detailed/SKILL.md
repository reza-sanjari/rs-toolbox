---
name: detailed
description: >
  One complete, full explanation of a topic, in simple and clear sentences.
  Applies to the next answer only. Invoke only with /rs:detailed.
disable-model-invocation: true
---

# detailed

Explain the topic completely, so the user fully understands it. Applies to **one answer only**.

## Which answer

- If the user wrote a question with the command, answer it now: $ARGUMENTS
- If nothing was written with the command, apply this to your answer to the user's next message. Confirm in a few words, for example "OK, next answer will be a full explanation."

After that one answer, go back to how you answered before (normal, or brief if brief mode is on).

## Complete means

- Cover everything the user needs to understand the topic: what it is, how it works, why, and what to watch out for. Leave out what does not help with the question.
- Include the important cases, limits, and common mistakes.
- Use an example when it makes something clearer.
- If there are several options, show them and say which one you recommend and why.

## Complete does not mean long-winded

1. **Simple, clear sentences.** One idea per sentence. Plain words. Explain a technical term the first time you use it.
2. **Say each thing once.** No repeating the same point in other words. No summary that repeats everything.
3. **No filler.** No greetings, no "Great question", no closing offer to help.
4. **Logical order.** Start with a short direct answer, then build up from basics to details.
5. **Structure for longer answers.** Use headings or lists when they make the answer easier to follow, not as decoration.
6. **Keep exact things exact.** Names, numbers, quotes, code, and commands stay complete and unchanged.
7. **User's language.** Answer in the language the user writes in.

## Before sending

1. Could the user still have an obvious follow-up question about this topic? Answer it.
2. Is any sentence repeating an earlier one? Delete it.
3. Is any sentence hard to read? Split it or simplify it.
