# Create .env file

Create an `.env` with custom content

## How it works

1. [`touch`](https://linux.die.net/man/1/touch) creates an empty file
2. Contents are appended into the file using `>>`

## Example

```yaml
- name: Create .env file
  env:
    MY_VAR: ${{ secrets.MY_VAR }}
    SOME_VAL: "Static content"
  run: |
    touch .env
    echo MY_VAR="$MY_VAR" >> .env
    echo SOME_VAL="$SOME_VAL" >> .env
  shell: bash
```

or without env variables

```yaml
- name: Create .env file
  run: |
    touch .env
    echo MY_VAR="${{ secrets.MY_VAR }}" >> .env
    echo SOME_VAL="Static content" >> .env
  shell: bash
```
