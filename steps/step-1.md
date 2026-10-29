# Step 1: Add the model call to the port

## The idea

Until now Coldline asked the model one kind of question: "summarize this". The agent needs a second one: "given where we are, what should happen next?". Asking it means adding a method to the model port, not building something new beside it.

Why the port? Because the port is the single promise every model adapter keeps. Today one adapter is the emulator. Later another will be a real hosted model. The worker and the agent reach the same model through the same port. If the agent had its own private way of asking, the next model you plug in would have to be wired twice, and the promise "Coldline has exactly five ports" would quietly stop being true.

Changing a promise has a useful side effect: everything that no longer keeps it gets named. That's what the contract check and the type checker are for. After you declare the new method, the emulator doesn't keep the promise yet, and the checks say so. That failure is the check doing its job. It tells you which adapters still need work before anything runs.

The method follows the pattern of the summary call exactly: it's a coroutine, it takes one typed request, and it returns one typed answer, the types you wrote in Task 1.

## What's expected

- In the Python shell, `from ports import ModelProvider` works, and `dir(ModelProvider)` lists both `summarize` and `propose`.
- `propose()` is declared `async`, takes a `ProposalRequest` and returns a `ProposedAction`, with a docstring that says what every adapter must guarantee.
- The port's class docstring describes both questions it now answers.
- `poe contract` still counts five ports. Its adapter test fails, naming the emulator, until Step 2.
- `poe typecheck` names every class used as a model provider that lacks the new method.

## When you're stuck

### You're not sure how to declare the call

<details><summary>Move 1</summary>

Open `src/ports/model_provider.py` and read how `summarize()` is declared: its keyword, its parameters, its return type and its docstring.

</details>

<details><summary>Move 2</summary>

Where do the request and answer types for `summarize()` come from? Find the import at the top of the file.

</details>

<details><summary>Move 3</summary>

The new method follows the same pattern with the types you wrote in Task 1. Import them from the module where you put the contract.

</details>

<details><summary>Move 4</summary>

The approach: an async method named `propose` with one request parameter of your request type, returning your proposal type. Its body is just the docstring, like the existing one. Then update the class docstring so it no longer says the port answers one question.

</details>

### `poe contract` fails and you don't know why

<details><summary>Move 1</summary>

Which test failed: the one that counts ports, or the one that checks adapters? Read the test's name in the output.

</details>

<details><summary>Move 2</summary>

If it's the adapter test, ask what you just promised on the port that the emulator doesn't do yet.

</details>

<details><summary>Move 3</summary>

If it's the port-count test, did you define a new `Protocol` and export it from the `ports` package? Coldline must still have exactly five.

</details>

### You want the full list of what's missing

<details><summary>Move 1</summary>

The adapter test names the emulator, not the method. Which command reads every place a model provider is used and checks it against the port?

</details>

<details><summary>Move 2</summary>

Run `poe typecheck` and read every line it prints. Note each class it names.

</details>

<details><summary>Move 3</summary>

For each class, ask whether it's an adapter that answers for itself, or one that wraps another adapter. Both need the method, but they implement it differently. You'll do both in Step 2.

</details>

### You're tempted to start a new port for the agent

<details><summary>Move 1</summary>

Will the agent and the worker talk to the same model? If they will, why would they need two promises about it?

</details>

<details><summary>Move 2</summary>

Would `poe contract` notice a second `Protocol` defined outside the `ports` package? If it wouldn't, who keeps the five-port rule?

</details>

<details><summary>Move 3</summary>

Keep one port with two methods. That's the rule in the lesson's Boundaries, and it's what lets the next model plug in once.

</details>
