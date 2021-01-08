# publish-to-public-ecr

Use this GithubAction to build and publish a Docker image to a public AWS ECR repository

## Requirements

A public ECR for this repo. The GH action will strip `(<org>/).*(-docker)` from the repo name so the target ECR repository will become e.g:

- Repo name: `acme/some-app`, ECR name: `some-app`
- Repo name: `acme/some-app-docker`, Repo name: `some-app`

## Usage

create `.github/workflows/main.yml` and paste:

```yaml
name: Build and publish to public ECR

# Controls when the action will run. 
on: [push, pull_request,workflow_dispatch]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Push to public ECR
      uses: citizensadvice/publish-to-public-ecr-github-action@main
      id: ecr
      with:
        aws-access-key-id: ${{ secrets.<aws key secret name> }}
        aws-secret-access-key: ${{ secrets.<aws secret key secret name> }}
        ecr-alias: citizensadvice
        # uncomment if you want main and master branches to be tagged with 'latest'
        #latest-tag-branches: main,master
```

The GH Actions generates these tags:

- `latest` for the most recent builds of the branches specified in `inputs.latest-tag-branches`
- `<branch_name>-<build_num>_<sha>` for PR and branch builds
- `<branch_name>` for the latest PR and branch builds
