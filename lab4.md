# Technique: Parent-Child

## Keyword to use: trigger & include 

## Parent Pipeline
### Syntax to add in your parent Pipeline.

### Parent Pipeline
```
stages:
  - build
build-job:
  stage: build
  script:
    - echo "I am parent pipeline job"
trigger-pipeline:
  stage: build
  trigger:
    include: <ChildFolder>/.gitlab-ci.yml
```

### Child Pipeline

```
stages:
 - unitest
run-unitest:
    stage: unitest
    script:
      - echo "I am child pipeline job"
```

# Technique: Multi-Project

## Get the Project name and group of which you want to trigger the pipeline.

## Update the .gitlab-ci.yml in the another Project with groupname and project name as follows

```
stages:
  - build
build-job:
  stage: build
  script:
    - echo "I am building the Job"
trigger-multproject:
  stage: build
  trigger:
    project: <GroupName>/<Project Name>
```
