# CI/CD Lab: Python App to Amazon ECS with Terraform and GitHub Actions

Two GitHub Actions pipelines: one provisions the AWS infrastructure with Terraform, the other
tests, scans, containerizes and deploys a Flask app to **Amazon ECS (Fargate)** behind a
**Network Load Balancer**.

> Lab from my MBA coursework. The step-by-step guides (in Portuguese) were written by
> Prof. Gerson Carneiro. I implemented and ran both pipelines in my AWS lab account through
> feature branches and pull requests: infra pipeline, SonarQube integration, Terraform fixes
> and network corrections.

## Pipelines

### Infrastructure: `.github/workflows/infra.yml` (branch `infra`)
Terraform (with TFLint) provisions:
- ECS cluster
- Network Load Balancer, listener and target group
- Security group with ingress rules for web and container traffic

![Infra pipeline](./Labs/doc-infra/img/cicd-infra.png)

### Application: `.github/workflows/app.yml` (branch `main`)
1. **Unit tests** (`unittest`) on the Flask app
2. **SonarQube Cloud** code-quality scan
3. **Docker build** and **Trivy** vulnerability scan
4. **Push** the image to Docker Hub
5. **Render and register** the ECS task definition with the new image
6. **Terraform** creates or updates the ECS service and CloudWatch log group (S3 state backend)
7. **Deploy** to ECS and wait for the service to stabilize

![App pipeline](./Labs/doc-app/img/cicd-app.png)

## Repository layout

| Path | Content |
|---|---|
| `app/` | Flask app, unit test, Dockerfile, SonarQube config |
| `app/deploy/` | Terraform for the ECS service and task definition |
| `infra/` | Terraform for cluster, NLB and security groups |
| `Labs/` | Step-by-step lab guides (PT-BR) |

## Guides (PT-BR)
- [Infrastructure pipeline](./Labs/doc-infra/README.md)
- [Application pipeline](./Labs/doc-app/README.md)
