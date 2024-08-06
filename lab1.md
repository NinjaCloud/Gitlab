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
  script:
    - echo npm install
    - echo npm test
  after_script:
    - echo "Execute after script section"

deploy_job:
  stage: deploy
  script:
    - echo "Deploying....my-app"
```

## Create a gitlab account 

## Create group in your gitlab acccount and add project

## Find Setup CICD in write side options or go to build and select pipeline editor

## Modify .gitlab-ci.yaml file

```
first_job:
  script:
    - echo "This is our first job"
    - ls
    - cat README.md
```

## commit the changes and see the status of pipeline

## Lets understand how to run jobs with shared runners

# first create or modify the new pipeline in the project with following content

```
windows_job:
  tags:
    - shared-windows
  script:
    - echo "Windows OS Version"
    - systeminfo

linux_job:
  tags:
    - saas-linux-medium-amd64
  script:
    - echo "linux OS Version"
    - cat /etc/os-release

macos_job:
  tags:
    - saas-macos-medium-m1
  script:
    - echo " MacOS OS Version"
    - system_profiler SPSoftwareDataType
```
