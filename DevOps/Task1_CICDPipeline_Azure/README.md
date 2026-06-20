# CI/CD Pipeline using Azure — CodeAlpha DevOps Internship

## Overview
An automated CI/CD pipeline built with **Azure Pipelines** that builds a Docker image from the portfolio web app and pushes it to **Azure Container Registry (ACR)** on every push to the `main` branch.

## Objectives Covered
- Build an automated CI/CD pipeline with Azure Pipelines
- Use Azure Container Registry for container storage
- Monitor pipeline execution
- Learn key DevOps concepts using Azure tools

## Tech Stack
- Azure DevOps (Pipelines)
- Azure Container Registry (Basic tier)
- Docker
- GitHub (source repo, integrated via Azure Pipelines GitHub App)

## Project Structure
```
Task1_CICDPipeline_Azure/
├── screenshots/
│   ├── 01_github_repo_with_pipeline_yaml.png
│   ├── 02_pipeline_build_success.png
│   ├── 03_pipeline_stage_details.png
│   └── 04_pipeline_run_summary.png
└── README.md
```

The pipeline definition (`azure-pipelines.yml`) lives at the **root of the repo**, since Azure Pipelines requires it there to detect commits and trigger automatically.

## Pipeline Configuration

```yaml
trigger:
- main

resources:
- repo: self

variables:
  dockerRegistryServiceConnection: 'e6c81872-7afe-488d-84d8-a554cb800559'
  imageRepository: 'teejancodealphatasks'
  containerRegistry: 'codealphadevopsacr.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/DevOps/Task_DockerWebServer/Dockerfile'
  tag: '$(Build.BuildId)'
  vmImageName: 'ubuntu-latest'

stages:
- stage: Build
  displayName: Build and push stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)
    steps:
    - task: Docker@2
      displayName: Build and push an image to container registry
      inputs:
        command: buildAndPush
        repository: $(imageRepository)
        dockerfile: $(dockerfilePath)
        containerRegistry: $(dockerRegistryServiceConnection)
        tags: |
          $(tag)
```

## How It Works

1. **Trigger** — Any push to the `main` branch automatically starts the pipeline.
2. **Agent** — An Ubuntu-hosted Microsoft agent spins up to run the job.
3. **Checkout** — The pipeline pulls the latest code from the GitHub repo.
4. **Build & Push** — Docker builds the image using the existing Dockerfile from the Task 4 (Docker Web Server) folder, tags it with the unique Build ID, and pushes it to Azure Container Registry.
5. **Registry** — The resulting image is stored in `codealphadevopsacr.azurecr.io` under the repository `teejancodealphatasks`.

## Setup Steps Followed

1. Created an Azure DevOps organization and project (`codealpha-cicd-pipeline`).
2. Connected the project to GitHub via the Azure Pipelines GitHub App, authorizing access to `teejan7/codealpha_tasks`.
3. Created an **Azure Container Registry** (`codealphadevopsacr`, Basic tier, Central India) under the Azure for Students subscription.
4. Selected the **Docker → Build and push an image to Azure Container Registry** pipeline template.
5. Configured the registry, image name, and Dockerfile path.
6. Committed the generated `azure-pipelines.yml` directly to the `main` branch.
7. Pipeline ran automatically and completed successfully.

## Proof of Execution

| Screenshot | Description |
|------------|--------------|
| `01_github_repo_with_pipeline_yaml.png` | GitHub repo showing `azure-pipelines.yml` committed by the pipeline setup |
| `02_pipeline_build_success.png` | Pipeline job logs — all steps (Initialize, Checkout, Build and push, Finalize) completed successfully |
| `03_pipeline_stage_details.png` | Stage details showing trigger source, commit, and condition evaluation |
| `04_pipeline_run_summary.png` | Run summary — Status: Success, Duration: 21s |

## Monitoring & Troubleshooting

- Pipeline run status, duration, and logs are viewable under **Pipelines → Runs** in Azure DevOps.
- A minor warning (`No data was written into the file .../task_outputs/...txt`) appeared in the run — this is a known harmless logging quirk of the Docker@2 task and does not affect the build or push outcome.
- Each run is tagged with a unique Build ID, so multiple image versions can coexist in the registry without overwriting each other.

## What I Learned

- How Azure Pipelines connects to GitHub repos and triggers automatically on commits.
- The difference between a pipeline **definition** (YAML) and a pipeline **run** (an execution instance).
- How Azure Container Registry stores and versions container images using tags.
- How to create and configure cloud resources (Container Registry, Resource Group) within the constraints of an Azure for Students subscription, including resolving a deployment policy validation error by adjusting the pricing tier and region.
- How CI/CD reduces manual steps — a single `git push` now results in a built and registry-stored image with no manual intervention.

## Status
✅ Completed and verified — pipeline runs successfully and pushes the image to Azure Container Registry on every commit to `main`.

---
**Intern:** Teejan Tee Pee | **Domain:** DevOps | **CodeAlpha Internship (June–July 2026)**
