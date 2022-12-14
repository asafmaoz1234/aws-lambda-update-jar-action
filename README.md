# AWS Lambda Update Jar/zip 
### (no hidden logic)

[![GitHub](https://img.shields.io/github/license/asafmaoz1234/aws-lambda-update-jar-action)](https://opensource.org/licenses/MIT)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/asafmaoz1234/aws-lambda-update-jar-action)](https://github.com/asafmaoz1234/aws-lambda-update-jar-action/releases)
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/asafmaoz1234/aws-lambda-update-jar-action/v1)](https://github.com/asafmaoz1234/aws-lambda-update-jar-action/actions)

This action updates a given lambda that runs java projects.<br>
This action packages your project jar and directly updates your aws lambda.<br>
* The code in dist/index.js is clear, you can see exatcly whats going on with your aws credentials.

## Usage

### complete

```yaml
- name: 'AWS Lambda Update Jar/zip'
  uses: asafmaoz1234/aws-lambda-update-jar-action@1.1
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

**Required** 

### `AWS_ACCESS_KEY_ID`

**Required**

### `AWS_SECRET_ACCESS_KEY`

**Required**

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
      - name: Update lambda
        uses: asafmaoz1234/aws-lambda-update-jar-action@v1
        with:
          lambda-name: my-lambda-name
          snapshot-name: project-name-1.0-SNAPSHOT.jar
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: ${{ secrets.AWS_REGION }}
```
