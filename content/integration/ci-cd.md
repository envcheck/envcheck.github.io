+++
title = "CI/CD Pipelines"
weight = 3
+++

# CI/CD Pipeline Integration

Integrate envcheck into your CI/CD pipeline for environment validation.

## GitLab CI

```yaml
stages:
  - validate
  - deploy

envcheck:
  stage: validate
  image: rust:latest
  script:
    - cargo install envcheck
    - envcheck lint .env.example .env.prod
    - envcheck compare .env.example .env.prod
    - envcheck k8s-sync k8s/**/*.yaml --env .env.example
  rules:
    - changes:
        - .env*
        - k8s/**/*.yaml

deploy:prod:
  stage: deploy
  script:
    - kubectl apply -f k8s/
  environment:
    name: production
  needs:
    - envcheck
```

## Jenkins

```groovy
pipeline {
    agent any

    stages {
        stage('Envcheck') {
            steps {
                sh 'cargo install envcheck'
                sh 'envcheck lint .env.example --format=json > envcheck.json'
                sh 'envcheck compare .env.example .env.prod'
                sh 'envcheck k8s-sync k8s/**/*.yaml --env .env.example'
            }
        }

        stage('Deploy') {
            needs: 'Envcheck'
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'envcheck.json'
        }
    }
}
```

## Azure Pipelines

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: UsePythonVersion@0
    inputs:
      versionSpec: '3.x'

  - script: |
      curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
      export PATH="$HOME/.cargo/bin:$PATH"
      cargo install envcheck
    displayName: 'Install envcheck'

  - script: |
      envcheck lint .env.example --format=json > envcheck.json
    displayName: 'Run envcheck'

  - task: PublishBuildArtifacts@1
    inputs:
      pathtoPublish: 'envcheck.json'
      artifactName: 'envcheck-report'
```

## CircleCI

```yaml
version: 2.1

orbs:
  rust: circleci/rust@1.1

jobs:
  envcheck:
    docker:
      - image: cimg/rust:latest
    steps:
      - checkout
      - run:
          name: Install envcheck
          command: cargo install envcheck
      - run:
          name: Lint env files
          command: envcheck lint .env.example
      - run:
          name: Compare environments
          command: envcheck compare .env.example .env.prod
      - run:
          name: K8s sync check
          command: envcheck k8s-sync k8s/**/*.yaml --env .env.example
      - store_artifacts:
          path: envcheck.json

  deploy:
    docker:
      - image: cimg/base:stable
    steps:
      - checkout
      - run: kubectl apply -f k8s/
    requires:
      - envcheck

workflows:
  version: 2
  validate-and-deploy:
    jobs:
      - envcheck
      - deploy:
          filters:
            branches:
              only: main
```

## Bitbucket Pipelines

```yaml
pipelines:
  default:
    - parallel:
        - step:
            name: 'Envcheck'
            image: rust:latest
            script:
              - cargo install envcheck
              - envcheck lint .env.example
              - envcheck compare .env.example .env.prod
              - envcheck k8s-sync k8s/**/*.yaml --env .env.example
            artifacts:
              - envcheck.json

  branches:
    main:
      - step:
          name: 'Validate and Deploy'
          script:
            - cargo install envcheck
            - envcheck doctor --all
            - kubectl apply -f k8s/
          deployment: production
```

## Drone CI

```yaml
kind: pipeline
type: docker
name: default

steps:
  - name: envcheck
    image: rust:latest
    commands:
      - cargo install envcheck
      - envcheck lint .env.example
      - envcheck compare .env.example .env.prod
      - envcheck k8s-sync k8s/**/*.yaml --env .env.example

  - name: deploy
    image: bitnami/kubectl:latest
    commands:
      - kubectl apply -f k8s/
    depends_on:
      - envcheck
    when:
      branch:
        - main
```

## Concourse CI

```yaml
resources:
  - name: envcheck-repo
    type: git
    source:
      uri: https://github.com/envcheck/envcheck.git

jobs:
  - name: validate-env
    plan:
      - get: envcheck-repo
        trigger: true
      - task: run-envcheck
        config:
          platform: linux
          image_resource:
            type: registry-image
            source:
              repository: rust
          inputs:
            - name: envcheck-repo
          outputs:
            - name: report
          run:
            path: sh
            args:
              - -c
              - |
                cd envcheck-repo
                cargo install envcheck
                envcheck doctor --format=json > ../report/envcheck.json
```

## Woodpecker CI

```yaml
pipeline:
  envcheck:
    image: rust:latest
    commands:
      - cargo install envcheck
      - envcheck lint .env.example
      - envcheck compare .env.example .env.prod
      - envcheck k8s-sync k8s/**/*.yaml --env .env.example

  deploy:
    image: bitnami/kubectl:latest
    commands:
      - kubectl apply -f k8s/
    when:
      branch: main
    depends_on:
      - envcheck
```

## See Also

- [GitHub Actions](github-actions.md)
- [Pre-commit Hooks](pre-commit.md)
