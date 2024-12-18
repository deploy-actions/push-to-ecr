# Build and Push Docker Image to Amazon ECR 🐳

## About

The Action builds and pushes a docker image with the specified image tag.

If the specified ECR Repository does not exist, create repository.

## Simple Example of Usage

```yml
- name: Configure AWS credentials 🔑
  uses: aws-actions/configure-aws-credentials@main
  with:
    role-to-assume: ${{ vars.AWS_ROLE_ARN }}
    aws-region: ${{ vars.AWS_REGION }}

- name: Build & Push Docker Image 🐳
  id: image
  uses: deploy-actions/push-to-ecr@v1
  with:
    repository-name: simple-api

- name: Deploy Lambda Function ƛ
  id: lambda
  uses: deploy-actions/mono-lambda
  with:
    function-name: simple-api
    image-uri: ${{ steps.image.outputs.image_uri }}
    function-url: true
    environment: |
      NODE_ENV=production
      DATABASE_URL=postgresql://admin@localhost:5432/simple-api

- run: echo "${{ steps.lambda.outputs.host }}"
```

## Options

| Name            | Description                                                          | mandatory | Default    |
| --------------- | -------------------------------------------------------------------- | --------- | ---------- |
| repository-name | ECR Repository Name                                                  | ✅        |            |
| image-tag       | Docker Image Tag                                                     |           | latest     |
| overwrite       | Overwrites the image, even if the image tag already exists.          |           | true       |
| dockerfile-path | Relative Path of Dockerfile, relative to the github repository root. |           | .          |
| dockerfile-name | Dockerfile Name                                                      |           | Dockerfile |
