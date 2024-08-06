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
       PASSWORD: password@123
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


### Explanation

**Workflow**:
- The pipeline is named "Generate ASCII Artwork."

**Stages**:
- **build**: Compiling or creating artifacts.
- **test**: Verifying the created artifacts.
- **docker**: Handling Docker image creation and pushing.
- **deploy**: Deploying the final product.

**Variables**:
- **USERNAME**: Specifies the Docker registry username (`dockerUsername`).
- **REGISTRY**: Defines the Docker registry path using the username (`docker.io/dockerUsername`).
- **IMAGE**: The name of the Docker image to be built.
- **VERSION**: Uses the pipeline ID for tagging the Docker image (`$CI_PIPELINE_ID`).

**.build_file**:
- **stage**: This job is in the `build` stage.
- **before_script**: Installs the `cowsay` gem before executing the main script.
- **script**: Uses `cowsay` to generate a dragon ASCII art with a custom message and saves it to `dragon.txt`.
- **artifacts**: Saves `dragon.txt` as an artifact named "Dragon Text File," available for 3 days if the job succeeds.

**.test_file**:
- **stage**: This job is in the `test` stage.
- **script**: Searches for the term "dragon" in `dragon.txt`, case-insensitively, to verify the content.

**.docker_build**:
- **stage**: This job is in the `docker` stage.
- **needs**: Depends on the completion of the `.build_file` job.
- **script**: Echoes the command to build a Docker image with the specified registry, image name, and version.

**.docker_testing**:
- **stage**: This job is in the `docker` stage.
- **needs**: Depends on the `.docker_build` job.
- **script**: Echoes the command to run the Docker container from the built image.

**docker_push**:
- **stage**: This job is in the `docker` stage.
- **variables**: 
  - **PASSWORD**: Placeholder for the Docker registry password.
- **script**: 
  - Echoes the command to log in to the Docker registry.
  - Echoes the command to push the Docker image to the registry.

**deploy_ec2**:
- **stage**: This job is in the `deploy` stage.
- **script**:
  - Displays the contents of `dragon.txt`.
  - Echoes a deployment message and Docker credentials (for illustrative purposes).
