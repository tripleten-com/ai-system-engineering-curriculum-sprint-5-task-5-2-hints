# Step 4: Prove it with tests and write the evidence note

## Your counter break doesn't fail any test

<details><summary>Hint 1</summary>

How many emulators does your in-order test build? If it builds a fresh one for each position, can it tell a counter from the request's position?

</details>

<details><summary>Hint 2</summary>

On one adapter, what request would give a different answer from an adapter that counts its own calls? Try asking for a position twice, or going backwards.

</details>

## The expected values in your tests drift from the scripts

<details><summary>Hint 1</summary>

Where do your tests get the values they compare against? If you typed them in, which copy is right when the script changes?

</details>

<details><summary>Hint 2</summary>

Load the script file in the test and compare each returned proposal with the entry at the same position.

</details>

## A wrong refusal still passes your test

<details><summary>Hint 1</summary>

What does your malformed-path test assert? Would it fail if the emulator refused for a different reason?

</details>

## Async tests don't run or always pass

<details><summary>Hint 1</summary>

Is the method you call `async`? What do you get back if you call it without awaiting it?

</details>

<details><summary>Hint 2</summary>

Look at how an existing async test in the repository is declared, and declare yours the same way.

</details>

## CI is green but the type checker isn't

<details><summary>Hint 1</summary>

`poe verify` doesn't run `poe typecheck` or `poe contract`. Run both after your last code change. Which classes does the type checker still name?

</details>
