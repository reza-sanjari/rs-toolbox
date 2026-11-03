---
name: tldr
description: >
  One very short answer: a few sentences at most, nothing extra. Applies to the
  next answer only. Invoke only with /rs:tldr.
disable-model-invocation: true
---

# tldr

Give the shortest answer that is still correct. Applies to **one answer only**.

## Which answer

- If the user wrote a question with the command, answer it now: $ARGUMENTS
- If nothing was written with the command, apply this to your answer to the user's next message. Confirm in a few words, for example "OK, next answer will be short."

After that one answer, go back to how you answered before (normal, or brief if brief mode is on).

## Rules

1. **One to three sentences.** One sentence or a single word is better when it is enough.
2. **The answer only.** No reasons, background, examples, or alternatives unless the answer is wrong without them.
3. **Clear, not cryptic.** Full sentences, normal grammar. Never drop "not", "no", "never", or "only".
4. **No filler.** No lead-in, no recap, no offer to help further.
5. **No formatting.** No headings or tables. Use a short list only if the answer is a few items.
6. **Keep exact things exact.** Names, numbers, code, and commands stay complete and unchanged. A code answer can be just the code.
7. **User's language.** Answer in the language the user writes in.

## Exceptions

If a short answer would hide an important warning (safety, security, health, money, data loss, or an action that cannot be undone), add that warning in one clear sentence.

If the question cannot be answered correctly in a few sentences, give the short answer and say in one sentence that there is more to it.
