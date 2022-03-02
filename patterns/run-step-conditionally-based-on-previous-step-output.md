# Run step conditionally based on previous step's output

Skip running a step if the previous step has specific output. For example when deploying infrastructure with AWS CDK,

## How it works

1. Write both `stdout` and `stderr` also to `output.log` with [`tee`](https://www.man7.org/linux/man-pages/man1/tee.1.html)
2. Check with [`grep`](https://www.man7.org/linux/man-pages/man1/grep.1.html) if the output contains a specific text and store it as the [output parameter](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#setting-an-output-parameter) from that step.
3. Run the step only if the text was found (value is 0) or not (value is 1)

## Example

```yaml
- name: Step whose output to check
  run: echo "Some command" 2>&1 | tee output.log

- name: Check if there were differences
  id: diff
  run: echo "::set-output name=outputcheck::$(grep "$TEXT_TO_FIND" output.log &> /dev/null; echo $?)"
  env:
    TEXT_TO_FIND: Output to check

- name: Run when the text was found
  if: ${{ steps.diff.outputs.outputcheck == '0') }}
  run: echo "This will run when the text was found"

- name: Run when the text was NOT found
  if: ${{ steps.diff.outputs.outputcheck == '1') }}
  run: echo "This will run when the text was NOT found"
```
