# Core Concept

## Understand the core concept of pipleline using following example

```
workflow:
  name: my-app
  rules: 
    - if: $CI_COMMIT_BRANCH == 'main'

stages:
  - test
  - deploy

unit_test-job:
  stage: test
  tags:
    - saas-linux-small-amd64
  before_script:
    - echo "Add cmds to install NodeJS"
  scripts:
    - echo npm install
    - echo npm test
  after_script:
    - echo "Execute after script section"

deploy_job:
  stage: deploy
  script:
    - echo "Deploying....my-app"
```
