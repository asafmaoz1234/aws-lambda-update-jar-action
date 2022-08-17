# AWS Lambda Update Jar/zip

[![GitHub](https://img.shields.io/github/license/asafmaoz1234/aws-lambda-update-jar-action)](https://opensource.org/licenses/MIT)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/asafmaoz1234/aws-lambda-update-jar-action)](https://github.com/asafmaoz1234/aws-lambda-update-jar-action/releases)
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/asafmaoz1234/aws-lambda-update-jar-action/v1)](https://github.com/asafmaoz1234/aws-lambda-update-jar-action/actions)

This action updates a given lambda that runs java projects.<br>
This action packages your project jar and directly updates your aws lambda.

## Usage

### minimum

```yaml
uses: asafmaoz1234/aws-lambda-update-jar-action@v1
with:
  lambda-name: 'lambda-name-to-update'
  snapshot-name: 'projectName-artifactId.jar'
```

### complete

```yaml
uses: asafmaoz1234/aws-lambda-update-jar-action@v1
with:
  lambda-name: 'lambda-name-to-update'
  snapshot-name: 'projectName-artifactId.jar'
  AWS_REGION: ${{ secrets.AWS_REGION }}
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## AWS Permissions needed
 - IAM user (prefebly not your root user) with (at least) lambda write permission (example - `"AWSLambdaRole"` policy attached)
 - Lambda updated with AWSLambdaRole `"lambda:UpdateFunctionCode"` permission for the user.


## Inputs

### `snapshot-name`

**Required**. The jar location, required argument of this action.

### `lambda-name`

**Required**. The lambda name, required argument of this action.

### `AWS_REGION`

_Optional_, if not specified fallbacks to environment variable.

### `AWS_ACCESS_KEY_ID`

_Optional_, if not specified fallbacks to environment variable.

### `AWS_SECRET_ACCESS_KEY`

_Optional_, if not specified fallbacks to environment variable.

## Example

### Java 8 Lambda - Update on merged pull request to main (production) branch

```yaml
name: Package and Deploy

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  build_and_update_if_merged:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 8
        uses: actions/setup-java@v3
        with:
          java-version: '8'
          distribution: 'corretto'
          cache: maven
      - name: Package to jar
        run: mvn package
      - name: Update lambda
        uses: asafmaoz1234/aws-lambda-update-jar-action@v1
        with:
          lambda-name: my-lambda-name
          snapshot-name: project-name-1.0-SNAPSHOT.jar
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: ${{ secrets.AWS_REGION }}
```
