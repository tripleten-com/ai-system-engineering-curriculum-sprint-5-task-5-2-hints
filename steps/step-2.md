# Step 2: Design your script format and make the emulator answer from it

## The idea

The emulator is a stand-in model whose answers you write in advance. For summaries it already builds its answers in code. For proposals you give it a script: a file with an ordered list of proposals, like lines in a play. A request names the script and the position, and the emulator returns the entry at that position.

Three ideas make that stand-in trustworthy:

- **Same request, same answer:** the position comes from the request, never from a counter the emulator keeps. Two runs can share one emulator without disturbing each other, and rerunning a run gives the same answers every time.
- **The contract decides what's valid:** the emulator doesn't check entries itself. It tries to build a proposal from each entry, and if the contract you wrote in Task 1 rejects it, the answer becomes a refusal with a fixed, named reason. A broken model answer stays visible as a refusal instead of being quietly repaired.
- **Setup problems aren't model answers:** a missing script, or a position past the end, means the test or run was set up wrong. Those raise an error, and the error type matters. The resilient wrapper around the emulator retries errors it thinks are temporary, so a setup error has to be one it passes straight through.

One practical constraint shapes the code: the worker image doesn't install the YAML parser, and the worker imports this module when it starts.

## What's expected

- Your well-behaved script exists in a format you designed, with a comment at the top saying what it plays and what happens when the entries run out.
- In the shell, the first position returns the first entry as a proposal, with the same kind, reason, count and tool input, and so on through the last.
- One position past the end, and a script name that doesn't exist, each raise the error you chose, and the resilient wrapper doesn't retry it.
- Reading your code, the refusal comes from catching the contract's rejection, with no checks of your own.
- `poe contract`, `poe typecheck` and `poe unit` pass, so the summary call is unchanged.
- `poe start` still brings the worker up.

## When you're stuck

### You can't decide on a script format

<details><summary>Hint 1</summary>

List the four requirements: one file per script, an ordered list of entries, the proposal's own field names, and a way for a reader to tell what the script plays.

</details>

<details><summary>Hint 2</summary>

Which file formats does the repository already read? Using one of them means no new parser, and your entries can be read straight into the proposal type.

</details>

<details><summary>Hint 3</summary>

How does a request point at a file? Decide how a script's name maps to a file name, so the request's script field is enough to find it.

</details>

<details><summary>Hint 4</summary>

The approach: one file per script, named after the script, holding a short description of what it plays and a list of entries. Each entry uses exactly the proposal's field names. The run-out behavior goes in a comment at the top.

</details>

### The worker container won't start after your change

<details><summary>Hint 1</summary>

Did the stack fail before your tests ran? Read the worker's log with `poe runtime-logs`.

</details>

<details><summary>Hint 2</summary>

Which libraries does the worker image install? Compare the worker's dependency group in `pyproject.toml` with what your new code imports.

</details>

<details><summary>Hint 3</summary>

Where in the file is the import? Code at the top of a module runs when the module is imported, which happens when the worker starts.

</details>

<details><summary>Hint 4</summary>

The approach: import the parser inside the function that reads a script, not at the top of the module. The worker never reads scripts, so it never runs that import.

</details>

### The emulator can't find your scripts

<details><summary>Hint 1</summary>

What folder is "here" when the code runs? Is it the same in your shell, in your tests and in a container?

</details>

<details><summary>Hint 2</summary>

Build the default location from the module's own file position, not from the folder you happen to be in.

</details>

<details><summary>Hint 3</summary>

Read `.dockerignore`. Does the image carry the folder where your scripts live? If not, the location has to be a setting a caller can change, with your default only for the shell and the tests.

</details>

### A second caller gets the wrong entry

<details><summary>Hint 1</summary>

Does your adapter store anything between calls? Search it for a value that changes each time the method runs.

</details>

<details><summary>Hint 2</summary>

Which field of the request already says which entry comes back?

</details>

<details><summary>Hint 3</summary>

The approach: read the script and pick the entry at the request's position on every call, with nothing remembered on the adapter.

</details>

### You're writing your own validity checks for each entry

<details><summary>Hint 1</summary>

Which code already knows every rule a proposal must follow? What happens when you build a proposal that breaks one?

</details>

<details><summary>Hint 2</summary>

If you check entries yourself, what happens the day someone changes a rule in the contract?

</details>

<details><summary>Hint 3</summary>

What does a refusal report as its count, and does it carry a tool input? Decide once, and use the same values for every refusal.

</details>

<details><summary>Hint 4</summary>

The approach: try to build the proposal from the entry exactly as written. If building it fails, return a refusal with the reason `malformed_proposal` and your chosen count. Never fix the entry and never read ahead.

</details>

### The wrapper retries a missing script

<details><summary>Hint 1</summary>

How does the resilient wrapper decide whether an error deserves another attempt? Read how it treats the error types in `src/domain/errors.py`.

</details>

<details><summary>Hint 2</summary>

Could a missing script fix itself on the next attempt? Which error type says "don't try again"?

</details>

<details><summary>Hint 3</summary>

Raise that type, or one of your own that extends it, for a missing script and for a position past the end. The wrapper then passes it straight to the caller.

</details>

### The emulator works on its own but not through the wrapper

<details><summary>Hint 1</summary>

Does the type checker name the wrapper? What does the wrapper do with a call it has no method for?

</details>

<details><summary>Hint 2</summary>

Look at how the wrapper handles the summary call: it applies a timeout and retries around the inner adapter's call.

</details>

<details><summary>Hint 3</summary>

The approach: give the wrapper its own proposal method that hands the request to the inner adapter under the same timeout and retry rules. The wrapper knows nothing about scripts.

</details>
