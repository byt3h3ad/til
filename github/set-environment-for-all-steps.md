# Set environment variables for all steps in a GitHub Action

From [this example](https://github.com/GoogleCloudPlatform/github-actions/blob/20c294aabd5331f9f7b8a26e6075d41c31ce5e0d/example-workflows/cloud-run/.github/workflows/cloud-run.yml) I learned that you can set environment variables such that they will be available in ALL jobs once at the top of a workflow:

```yaml
name: Build and Deploy to Cloud Run

on:
  push:
    branches:
    - master

env:
  PROJECT_ID: ${{ secrets.RUN_PROJECT }}
  RUN_REGION: us-central1
  SERVICE_NAME: helloworld-nodejs
```
