# test-build

## Step 1
Create a personal access token with `repo` and `workflow` permissions ready.

## Step 2
Create a workflow with sample code below and add a file call `index.js` with `console.log(main branch)`

```yaml
name: Build

on:
  repository_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: echo github actor
        run: echo ${{ github.actor }}

      - name: Checkout code
        uses: actions/checkout@v2
        with:
          ref: ${{ github.event.client_payload.branch || 'dev' }}

      - name: Run index.js
        run: node index.js
```
```bash
git add .
git commit -m "init"
git push origin main
```

## Step 3
Create a branch call `dev`, then commit `index.js` to `console.log(dev branch)`

```bash
git add .
git commit -m "fix: dev"
git push origin dev
```
## Step 4
Use `curl` to call dispatch action

```bash
## Example 1
curl --request POST \
  --url 'https://api.github.com/repos/ookangzheng/test-build/dispatches' \
  --header 'authorization: Bearer <PERSONAL_ACCESS_TOKEN>' \
  -d '{"event_type":"trigger", "client_payload": {"branch":"dev"}}'
  
## Example 2
curl -i -u <GITHUB_USERNAME>:<PERSONAL_ACCESS_TOKEN> --request POST \
  --url 'https://api.github.com/repos/ookangzheng/test-build/dispatches' \
  -d '{"event_type":"trigger", "client_payload": {"branch":"dev"}}'
```
