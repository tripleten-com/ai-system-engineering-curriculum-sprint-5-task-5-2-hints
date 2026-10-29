# Step 2: Design your script format and make the emulator answer from it

## The worker container won't start after your change

<details><summary>Hint 1</summary>

Which step of `poe verify` failed: did the stack fail to start before your tests ran? Read the worker's log with `poe runtime-logs`.

</details>

<details><summary>Hint 2</summary>

What does the worker import when it starts, and which libraries does its image install? Compare the worker's dependency group in `pyproject.toml` with what your new code imports, and where in the file it imports it.

</details>

## The emulator can't find your scripts

<details><summary>Hint 1</summary>

What folder is "here" when the code runs? Is it the same in your shell, in your tests and in a container?

</details>

<details><summary>Hint 2</summary>

Build the default location from the file's own position, not from the folder you happen to run in. Then check whether the containers carry that folder at all: read `.dockerignore`.

</details>

## A second caller gets the wrong entry

<details><summary>Hint 1</summary>

Does your adapter remember anything between calls? Search it for a value that changes each time `propose()` runs.

</details>

<details><summary>Hint 2</summary>

Which fact in the request already says which entry comes back? If the request carries it, what does the adapter still need to remember?

</details>

## The wrapper retries a missing script several times

<details><summary>Hint 1</summary>

How does the resilient wrapper decide whether an error is worth another attempt? Read how it treats the errors in `src/domain/errors.py`.

</details>

<details><summary>Hint 2</summary>

Is a missing script, or a position past the end, something that could fix itself on the next attempt? Which kind of error says "don't try again"?

</details>

## You're writing your own validity checks for each entry

<details><summary>Hint 1</summary>

Which code already knows every rule a proposal must follow? What happens when you build a proposal that breaks one?

</details>

<details><summary>Hint 2</summary>

Instead of checking first, try building the proposal and decide what to do when building it fails. What does a refusal report as its count, and does it carry an input? Make that the same for every refusal.

</details>

## The emulator works on its own but not through the wrapper

<details><summary>Hint 1</summary>

What does the resilient wrapper do with a call it doesn't have a method for? Does the type checker name it?

</details>

<details><summary>Hint 2</summary>

The wrapper adds a timeout and retries around whatever it wraps. Look at how it handles the summary call. What should it know about scripts?

</details>
