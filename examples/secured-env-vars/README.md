# Securing sensitive environment variables

This guide explains how to secure environment variables when using the Atlantis module on Google Cloud Platform. For more information on using this module, see the [`basic example`](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/basic).

## Prerequisites

You should already have the following resources:

- An Artifact or Container Registry in Google Cloud.
- A CI/CD system with a secret manager integration (such as GitHub, Gitlab, Jenkins, or Cloud Build).

## How to deploy

To deploy the Atlantis module, see [`Dockerfile`](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/secured-env-vars/Dockerfile) and the [`main.tf`](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/secured-env-vars/main.tf).

## Configuring Atlantis

Atlantis allows you to configure everything using environment variables. However, these variables may contain sensitive values, and are therefore visible in the Google Cloud console when deploying a container. To protect these values, follow the steps below.

### Setting sensitive environment variables

Use a wrapper Atlantis Docker image to set environment variables that contain sensitive values. See the following examples for more details:

- [**Cloud Build**: pull secrets from Google Secret Manager](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/secured-env-vars/cloudbuild.yaml)
- [**GitHub Actions**: pull secrets from Google Secret Manager](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/secured-env-vars/.github/workflows/docker-gcp-secrets.yaml)
- [**GitHub Actions**: use GitHub secrets](https://github.com/bschaatsbergen/atlantis-on-gcp-vm/tree/master/examples/secured-env-vars/.github/workflows/docker-github-secrets.yaml)

### Setting non-sensitive environment variables

Use the `var.env_vars` variable to set non-sensitive environment variables.

```hcl
env_vars = {
  ATLANTIS_EXAMPLE = "example"
}
```

> **Important**: Do **not** specify the same environment variable in both the env_vars and the Dockerfile, as this will cause the deployment to fail.
