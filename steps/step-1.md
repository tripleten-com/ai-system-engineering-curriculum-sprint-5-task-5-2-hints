# Step 1: Add the model call to the port

## `poe contract` fails and you don't know why

<details><summary>Hint 1</summary>

Which test failed: the one that counts ports, or the one that checks adapters? Read the test's name in the output.

</details>

<details><summary>Hint 2</summary>

If it's the adapter test, that's expected at this point. What did you just promise on the port that the emulator doesn't do yet?

</details>

## You want the full list of what's missing

<details><summary>Hint 1</summary>

The adapter test names the emulator, not the method. Which command reads every place a model provider is passed and checks it against the port?

</details>

<details><summary>Hint 2</summary>

Read every line that command prints and note each class it names. Is every one of them an adapter you'll extend in this Task, or does one only wrap another adapter?

</details>

## You're tempted to start a new port for the agent

<details><summary>Hint 1</summary>

Will the agent and the worker talk to the same model? If they do, why would they need two promises about it?

</details>

<details><summary>Hint 2</summary>

Would `poe contract` notice a second Protocol defined outside the `ports` package? If it wouldn't, who keeps the five-port rule?

</details>
