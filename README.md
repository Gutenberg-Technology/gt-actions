# GT Actions

This is repository is a collection of reusable workflows for the GT elements

## `deploy-frontend.yml`

### Features

- Sync new built file(s) to the related S3
- Invalidate the S3 cache
- Create GitHub Deployment (and update status during the Workflow, related steps ommited in scenario)

### Requirements

- `${{ inputs.working-directory }}/.aws/${{ inputs.env }}.env` file with:
  - `S3_BUCKET`: S3 bucket where the will be hosted
  - `S3_BUCKET_REGION`: AWS Region of the S3
  - `DIST_ID`: Cloudfront Distribution ID
  - `DIST_PATH`: Local path where Distribution is build during the CI workflow
- A built distribution uploaded to GitHub
- AWS secrets for S3 & Cloudfront steps
- GitHub secrets to manage Deployment status

### Scenario

- Checkout code & read env vars from `${{ inputs.working-directory }}/.aws/${{ inputs.env }}.env`
- Download the built archive & untar it
- Configure AWS Creds
- Deploy the built site in `DIST_PATH` to S3 & invalidate the Cloudfront cache

## `deploy-ecs-td.yml`

### Features

- Deploy a new version for a given Task Definition in AWS ECS using a valid task definition file from an uploaded artifact
- Create GitHub Deployment and update status during the Workflow (related steps ommited in scenario below)

### Requirements & inputs

- A valid task definition file as an uploaded artifact
- `env`: Env target to deploy the Task definition
- `tagged-image`: Full image name with its tag
- `ecs-cluster`: The target cluster, example: `abn-${{ github.event.inputs.env }}`
- `ecs-service`: The service where the container will be run
- `ecs-task-def`: A valid task definition file
- `container-name`: Name of your container in ECS.

### Scenario

- Download task definition artifact
- Configure AWS Creds & login to ECR
- Fill the Task Definition with new `tagged-image`
- Deploy new Task Definition to AWS

## `build-test-push-image.yml`

### Requirements

- A `Dockerfile` in your working directory
- Optional container structure tests config file
- AWS Secrets

### Scenario

- Configure AWS Creds & ECR Registry
  - Used to define full `target` name and push image to AWS ECR
- `docker build` image with `latest` and `GIT_SHA` tags
- [Optional] Test image with `container-structure-test` for both `latest` and `GIT_SHA:0:7` tags (requires config file)
- [Optional] Push all build tags to ECR

### Outputs

- `pushed-image` : the full image name built with its tag (`GIT_SHA:0:7`), example : `my-cool-img:1234567`
