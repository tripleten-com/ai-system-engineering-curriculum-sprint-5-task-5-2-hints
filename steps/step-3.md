# Step 3: Write the script that breaks the rules

## The idea

A real model sometimes answers with something that isn't quite a proposal: a kind that doesn't exist, a tool call with nothing to call, a count that makes no sense. Your well-behaved script can't show what Coldline does then. A malformed script can. It's a list of the mistakes you expect, each one written down so a test can prove Coldline refuses it with a name.

Two ideas make that script useful:

- **One mistake per entry:** each malformed entry breaks exactly one rule your contract enforces, with a comment naming the rule. When a test fails, you know which rule broke.
- **Valid entries around the mistakes:** a malformed entry must turn into one refusal, and the very next position must come back exactly as written. Without a valid entry after a mistake, nothing shows the emulator carried on correctly instead of stalling or skipping.

The mistake belongs in a proposal's fields, not in the file. A file the parser can't read tests the parser, which is a setup problem, not a model mistake. And a valid refusal in this script needs a reason of its own. If it used the emulator's malformed reason, nobody could tell the model's refusal from the emulator's.

## What's expected

- Your malformed script is readable by the emulator and has at least three malformed entries, each breaking a different contract rule, with a one-line comment naming it.
- There's at least one valid entry before the first malformed one and one after the last.
- In the shell, each malformed position comes back as a refusal with the reason `malformed_proposal`, and each valid position comes back exactly as written.
- Any valid refusal in the script uses its own reason.

## When you're stuck

### A malformed entry comes back as written

<details><summary>Hint 1</summary>

Is the entry actually malformed under your contract? Try building a proposal from exactly those fields in the shell.

</details>

<details><summary>Hint 2</summary>

If building it succeeds, the entry breaks a rule your contract doesn't have. Which rules does your contract enforce? Your Task 1 tests list them.

</details>

<details><summary>Hint 3</summary>

If building it fails but the emulator still returns the entry, go back to how the emulator handles a rejected entry in Step 2.

</details>

<details><summary>Hint 4</summary>

Pick mistakes from the lesson's list that your contract enforces, such as an unknown kind, an input on a refusal, a tool call without input, a missing field or a negative count. Then confirm each one in the shell.

</details>

### The file won't load at all

<details><summary>Hint 1</summary>

Does the error come from the parser or from the contract? Read where the error is raised.

</details>

<details><summary>Hint 2</summary>

Is the mistake in a proposal's fields, or in the file's syntax? A broken file tests whether the file can be read, not how Coldline handles a model's mistake.

</details>

<details><summary>Hint 3</summary>

Keep the file valid and put each mistake inside one entry's fields.

</details>

### You can't show the emulator recovers

<details><summary>Hint 1</summary>

What comes right after your last malformed entry?

</details>

<details><summary>Hint 2</summary>

If the script ends on a mistake, what would show that the next call isn't stuck or skipped?

</details>

<details><summary>Hint 3</summary>

Put a valid entry before the first mistake and another after the last, then walk every position in the shell and compare each one with the file.

</details>

### A valid refusal looks just like a malformed one

<details><summary>Hint 1</summary>

What reason does your valid refusal give?

</details>

<details><summary>Hint 2</summary>

If two refusals carry the same reason, can a reader tell which one the model chose and which one the emulator made?

</details>

<details><summary>Hint 3</summary>

Give the valid refusal a reason in the model's own words, so `malformed_proposal` only ever means "the emulator couldn't read the answer".

</details>
