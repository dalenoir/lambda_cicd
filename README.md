AWS Lambda CI/CD Pipeline with GitHub Actions

## Project Overview

This project demonstrates a basic DevOps CI/CD workflow using GitHub Actions to automatically deploy Python code to AWS Lambda whenever changes are pushed to the `main` branch.

The goal was to practice source control, GitHub Actions automation, AWS authentication through repository secrets, and Lambda deployment using the AWS CLI.

## Architecture Flow

```text
VS Code
→ Git add
→ Git commit
→ Git push
→ GitHub Repository
→ GitHub Actions Workflow
→ AWS Credentials
→ Zip Lambda Code
→ Deploy to AWS Lambda
```

## Tools and Services Used

- Git
- GitHub
- GitHub Actions
- VS Code
- AWS Lambda
- AWS IAM
- GitHub Repository Secrets
- Python 3.12
- YAML
- AWS CLI

## Project Structure

```text
lambda_cicd/
├── README.md
├── .github/
│   └── workflows/
│       └── lambda-deployment.yaml
└── lambda/
    ├── lambda_function.py
    └── requirements.txt
```

## Lambda Function

The Lambda function is written in Python and returns a simple JSON response.

```python
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello update lambda vscode')
    }
```

## GitHub Actions Workflow

The workflow is triggered when code is pushed to the `main` branch and changes are made inside either the `lambda/` folder or the `.github/workflows/` folder.

The workflow performs the following actions:

1. Checks out the repository code.
2. Sets up Python 3.12.
3. Installs dependencies if `requirements.txt` has content.
4. Configures AWS credentials using GitHub repository secrets.
5. Zips the Lambda code.
6. Deploys the updated code to AWS Lambda.

## GitHub Secrets Used

The following secrets were added to the GitHub repository:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

These secrets allow GitHub Actions to authenticate with AWS securely.

## Challenges Faced

### SSH Authentication Issue

At first, GitHub rejected the SSH connection with:

```text
Permission denied (publickey)
```

This was fixed by adding the local machine's SSH public key to GitHub under SSH and GPG keys.

### Git Commit Message Error

A commit command was run without quotes:

```bash
git commit -m intial setup for lambda and github actions
```

Git interpreted the extra words as file paths. This was fixed by using quotes around the commit message:

```bash
git commit -m "initial setup for lambda and github actions"
```

### Unclosed Quote Issue

The terminal entered the `quote>` prompt because of an unfinished quote. This was fixed by exiting with `Control + C` and rerunning the command properly.

### Requirements File Issue

The GitHub Actions workflow failed because `requirements.txt` contained the text:

```text
this file is empty
```

Pip treated that text like a package name and failed. The file was cleared using:

```bash
: > lambda/requirements.txt
```

After clearing the file and pushing the change, the workflow passed.

### Runtime Alignment

The AWS Lambda runtime was changed to Python 3.12 to match the GitHub Actions workflow Python version.

## Final Result

The pipeline successfully deployed updated Python code from GitHub to AWS Lambda.

The successful workflow proved that code changes pushed from VS Code can automatically trigger GitHub Actions and update the AWS Lambda function.

## What I Learned

- How to clone a GitHub repository locally.
- How to use Git add, commit, status, and push.
- How to configure SSH authentication with GitHub.
- How to create a GitHub Actions workflow.
- How to structure workflow files inside `.github/workflows/`.
- How to use GitHub repository secrets for AWS credentials.
- How to package Lambda code in a CI/CD pipeline.
- How to deploy AWS Lambda code using GitHub Actions.
- How to debug failed GitHub Actions runs.
- How to read workflow logs and fix pipeline errors.

## Resume Bullet

Built and debugged a CI/CD pipeline using GitHub Actions to automatically package and deploy a Python AWS Lambda function from a GitHub repository, integrating Git version control, GitHub repository secrets, AWS IAM credentials, and Lambda deployment automation.
