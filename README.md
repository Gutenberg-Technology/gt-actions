# GT Actions

This is repository is a collection of reusable workflows for the GT elements

## `deploy-frontend.yml`

### Features

- Sync new built file(s) to the related S3
- Invalidate the S3 cache

### Requirements

- The branch's `git-ref` you want to deploy
- Built archive uploaded to GitHub
- AWS Secrets

### Scenario

- Create GitHub Deployment (and upadate status during the Workflow)
- Download the built archive for the given `git-ref` and uploads artifact to GitHub
- Configure AWS Creds
- Deploy the built site to S3
