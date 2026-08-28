# jenkins-shared-lib

Reusable Jenkins pipeline functions for building, pushing, and deploying
the three-tier app (React + Node.js + MongoDB) across multiple
environments (dev, staging, prod).

## Functions (vars/)

| Function | Purpose |
|---|---|
| `checkoutSource()` | Checks out source code from SCM |
| `dockerLogin(credentialsId)` | Logs in to Docker Hub using Jenkins credentials |
| `buildImages(backendPath, frontendPath, tag)` | Builds backend & frontend Docker images |
| `pushImages()` | Pushes built images to the registry |
| `prepareEnvFile()` | Generates a `.env` file with image tags for Docker Compose |
| `deployApp(composeFile, branch)` | Deploys the app via Docker Compose |
| `cleanupImages()` | Removes local images after deployment |

## Usage

In a Jenkinsfile:

\`\`\`groovy
@Library('sharedLib') _

pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkoutSource() } }
        stage('Login') { steps { dockerLogin('dockerhub-credentials') } }
        stage('Build') { steps { buildImages('./backend', './frontend', 'v1') } }
        stage('Push') { steps { pushImages() } }
        stage('Deploy') { steps { deployApp('docker-compose.yml', 'dev') } }
    }
}
\`\`\`

Register this repo in Jenkins under
**Manage Jenkins → System → Global Pipeline Libraries** with the name `sharedLib`.
