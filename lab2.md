# Understand How to install third party libraries using before_script

## Create a new project of modify the .gitlab-ci.yaml file in the same project

```
workflow: 
  name: Generate ASCII Artwork from text

artwork_job:
  script:
    - echo "We are Generating ASCII Artwork using COWSAY Program"
    - cowsay -f dragon "Run for Cover, I am a DRAGON" > dragon.txt
    - grep -i "dragon" dragon.txt
    - cat dragon.txt
```

## commit the changes and see the status of pipeline

## If the pipeline status is showing faild find out the reason and fix it

```
workflow: 
  name: Generate ASCII Artwork from text

artwork_job:
  before_script:
    - gem install cowsay
  script:
    - echo "We are Generating ASCII Artwork using COWSAY Program"
    - cowsay -f dragon "Run for Cover, I am a DRAGON" > dragon.txt
    - grep -i "dragon" dragon.txt
    - cat dragon.txt
  after_script:
    - echo "This is the End of Pipeline"
```

## Now Lets Understand how to execute Shell scripts file in the job

### Go to Repo and create a new file called "script.sh" with follwoing commands

```
#!/bin/sh
echo "We are Generating ASCII Artwork using COWSAY Program"
cowsay -f dragon "Run for Cover, I am a DRAGON" > dragon.txt
grep -i "dragon" dragon.txt
cat dragon.txt
```

## Now Modify the pipeline and mention your script file

```
workflow: 
  name: Generate ASCII Artwork from text

artwork_job:
  before_script:
    - gem install cowsay
    - chmod +x script.sh
  script:
    - ./script.sh
  after_script:
    - echo "This is the End of Pipeline"
```

## Commit the changes and see the status of Pipeline





