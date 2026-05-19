# ShopCloud — Cloud-Native E-Commerce DevOps Project

## Overview
A cloud-native e-commerce frontend platform demonstrating enterprise DevOps practices including Docker containerization, GitHub Actions CI/CD pipelines, and Kubernetes orchestration.

## Team
| Name | Role | Contribution |
|------|------|-------------|
| Ahmed [LastName] | Team Lead | Home page, CI pipeline, Kubernetes manifests, repo structure |
| Faisal [LastName] | Developer | Product page, CD pipeline |

## Repository Structure:
cloud-native-ecommerce-devops/
├── .github/workflows/
│   ├── ci.yml
│   └── cd-deploy.yml
├── src/frontend/
├── k8s/
├── Dockerfile
├── Jenkinsfile
└── nginx.conf

## Environments
| Environment | Branch | URL |
|-------------|--------|-----|
| Production | `main` | https://shopcloud-prod.onrender.com |
| Staging | `staging` | https://shopcloud-staging.onrender.com |
| Development | `develop` | https://shopcloud-dev.onrender.com |

## CI/CD Pipeline
**CI** triggers on every push/PR:
1. HTML lint via htmlhint
2. Docker image build and container smoke test

**CD** triggers on push to `develop`, `staging`, or `main`:
1. Build and tag Docker image
2. Push to Docker Hub
3. Trigger Render deployment via API

## Local Development
```bash
docker build -t shopcloud:local .
docker run -p 8080:80 shopcloud:local
```

## Reflection
This project gave us hands-on experience with real CI/CD pipelines and multi-environment deployments. The trickiest part was correctly scoping GitHub Environment secrets so each branch deploys to the right Render service. Managing Docker image tagging across environments required careful planning. We learned how Git Flow enforces discipline in a collaborative team workflow.