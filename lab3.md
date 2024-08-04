# Working with variables at different level and create Simple Docker pipeline

## Create a simple docker pipeline 

```
workflow:
  name: Generate ASCII Artwork

stages:
   - build
   - test
   - docker
   - deploy

variables:
   USERNAME: dockerUsername
   REGISTRY: docker.io/$USERNAME
   IMAGE: imageName
   VERSION: $CI_PIPELINE_ID

build_file:
   stage: build
   before_script:
      - gem install cowsay
   script:
      - >
         cowsay -f dragon "Run for cover, 
         I am a DRAGON....RAWR" >> dragon.txt
   artifacts:
      name: Dragon Text File
      paths:
         - dragon.txt
      when: on_success
      expire_in: 3 days

test_file:
   stage: test
   script:
      - |
         grep -i "dragon" dragon.txt

docker_build:
    stage: docker
    needs:
       - build_file
    script:
      - echo "docker build -t $REGISTRY/$IMAGE:$VERSION"

docker_testing:
    stage: docker
    needs:
       - docker_build
    script:
      - echo "docker run -p 80:80 $REGISTRY/$IMAGE:$VERSION"

docker_push:
    stage: docker
   #  needs:
   #     - docker_testing
    variables:
       PASSWORD: $DOCKER_PASSWORD
    script:
      - echo "docker login --username=$USERNAME --password=$PASSWORD"
      - echo "docker push $REGISTRY/$IMAGE:$VERSION"

deploy_ec2:
   stage: deploy
   script:
      - cat dragon.txt
      - echo "deploying ... .. ."
      - echo "Username - $USERNAME and Password - $DOCKER_PASSWORD"
```
