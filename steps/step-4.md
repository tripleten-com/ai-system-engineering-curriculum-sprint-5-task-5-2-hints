# Step 4: Prove it with tests and write the evidence note

## The idea

Your emulator now makes three promises, and each one needs a test that would catch it breaking:

- **It plays the script as written:** every position returns the entry at that position, with every field equal to the file.
- **It turns a broken entry into a named refusal, then carries on:** a malformed position returns a refusal with the right reason, and the next position returns its entry.
- **It keeps no position of its own:** the answer depends only on the request.

The third promise is the easiest to test badly. A test that builds a fresh emulator for each position can't tell whether the emulator counts its own calls, because a fresh counter starts at the beginning every time. Only a test that sends several requests to one emulator, out of order or repeating a position, notices a counter.

Two habits keep the tests honest. Read the expected values from the script files, so a test and its data can't drift apart. And assert the reason, not just the kind, so a refusal for the wrong reason fails. As in Task 1, break each promise once on purpose and watch its test fail before you trust it.

> Covered in **CI Gates and Release Verification**.

## What's expected

- Your test file sits where `poe student-tests` collects it, beside your Task 1 tests, and both sets pass.
- One test walks the whole well-behaved script on a single emulator and compares every field with the file.
- The malformed tests assert both the kind and the reason, and check the entry after each mistake.
- One test uses a single emulator for requests out of order or repeated, and gets the entry each request names.
- You broke each of the three promises once and saw a test fail each time.
- `poe verify` passes after your last code change, and `poe contract` and `poe typecheck` pass too.

## When you're stuck

### Your counter break doesn't fail any test

<details><summary>Move 1</summary>

How many emulators does your in-order test build? One per position, or one for the whole test?

</details>

<details><summary>Move 2</summary>

With a fresh emulator per position, what would a counter inside the emulator return? Would that differ from the correct answer?

</details>

<details><summary>Move 3</summary>

On a single emulator, which sequence of requests would give a different answer from an emulator that counts its own calls? Try asking for the same position twice, or going backwards.

</details>

<details><summary>Move 4</summary>

The approach: one test, one emulator, a short sequence of positions that isn't simply 0, 1, 2, each compared with the file. Then add a counter to the emulator and watch that test fail while the in-order walk still passes.

</details>

### The expected values drift from the scripts

<details><summary>Move 1</summary>

Where do your tests get the values they compare with? Did you type them in?

</details>

<details><summary>Move 2</summary>

If someone edits a script, which copy is right: the file or the test?

</details>

<details><summary>Move 3</summary>

Load the script file inside the test and compare each returned proposal with the entry at the same position.

</details>

### A wrong refusal still passes your test

<details><summary>Move 1</summary>

What does your malformed-path test assert? Would it fail if the emulator refused with a different reason?

</details>

<details><summary>Move 2</summary>

Make the emulator refuse with another reason and run your tests. Which one fails?

</details>

<details><summary>Move 3</summary>

Assert the kind and the reason, and check the entry right after the mistake comes back as written.

</details>

### Async tests don't run, or always pass

<details><summary>Move 1</summary>

Is the method you call a coroutine? What do you get back if you call it without awaiting it?

</details>

<details><summary>Move 2</summary>

Find an existing async test in the repository and look at how it's declared and how it awaits.

</details>

<details><summary>Move 3</summary>

Declare your tests the same way and await every call. A coroutine object that's never awaited is always truthy, so a test that only checks it exists always passes.

</details>

### CI is green but the type checker isn't

<details><summary>Move 1</summary>

Does `poe verify` run `poe typecheck` or `poe contract`?

</details>

<details><summary>Move 2</summary>

Run both after your last code change. Which classes does the type checker still name?

</details>

<details><summary>Move 3</summary>

Give each class it names the new method, rerun both checks, then cite the final `poe verify` run in your note.

</details>
