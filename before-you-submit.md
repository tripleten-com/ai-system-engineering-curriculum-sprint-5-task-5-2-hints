# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Did you meet the Task's requirements?

- Is `propose()` on the existing port, with no second Protocol anywhere in the repository?
- Does every answer come from a script entry, apart from the malformed refusal?
- Does the emulator keep no position of its own between calls?
- Do `summarize()`, the inherited unit tests and your Task 1 tests still pass?

## Did you explain your design choices?

- Does your note describe your script format and why you chose it?
- Does it say what a refusal reports as its count, and why?
- Does it say what happens when a script runs out or doesn't exist, and why that isn't a refusal?

## Can your tests fail when the code is wrong?

- Did you break the adapter, the malformed path and the no-position rule once each, and see a test fail each time?
- Does at least one test use a single emulator for several requests, out of order?

## Did you state the limits of what your checks show?

- Does your note say what a scripted emulator can't tell you about a real model's answers?
