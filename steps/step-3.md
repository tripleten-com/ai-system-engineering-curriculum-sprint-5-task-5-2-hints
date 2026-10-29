# Step 3: Write the script that breaks the rules

## A malformed entry comes back as written

<details><summary>Hint 1</summary>

Is the entry actually malformed under your contract? Try building a proposal from exactly those fields in the Python shell.

</details>

<details><summary>Hint 2</summary>

If the contract accepts it, the entry breaks a rule your contract doesn't have, so pick a rule it enforces. If the contract rejects it but the emulator returns it anyway, go back to how the emulator handles a rejection.

</details>

## The file won't load at all

<details><summary>Hint 1</summary>

Is the mistake in a proposal's fields, or in the file itself? A file the parser can't read tests the parser, not the contract.

</details>

## A valid refusal looks just like a malformed one

<details><summary>Hint 1</summary>

What reason does your valid refusal give? If a reader sees two refusals with the same reason, can they tell which one the model chose and which one the emulator made?

</details>

## You can't show the emulator recovers

<details><summary>Hint 1</summary>

What comes right after your last malformed entry? Without a valid entry there, what would show that the next call isn't stuck or skipped?

</details>
