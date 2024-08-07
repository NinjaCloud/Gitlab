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

