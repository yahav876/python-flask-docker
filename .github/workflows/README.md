# Introduction

Hi , Here i will show you how to build a CI proccess with Github-Actions , In this senario we have a python app repository that contains all the files needed and we want to implement it in a test environemnt to check if everything is ok.


# Problem description

Automate CI process of python app with github actions using dokcer.

# Requirements

1. Fork this repository to your github account - https://github.com/lvthillo/python-flask-docker
2. Go to Settings > Actions and make sure "Allow all actions" is checked.
3. In order to github-actions will work we need to create a folder called .github/workflows
4. In this CI we need to make a login to our docker-hub account and push and pull the image from, For that we need to create a secret repository under Settigns > Secrets > New repository secret , put your secret name and value and click save.

# Solution

### 1. Building a simple workflow named main.yaml

- Name - Is the name of your workflow "Python application" in our case.
- on: push - means on each push action the workflow will execute.
- env: is where we declare the secrets we configured earlier ($USER and $PASSWD).
<<<<<<< HEAD
- jobs: <br />
       build - We create a job called "Build" where we building the docker image and push it.<br />
       run - We pull the image and run the container with few tests. <br />
       Note - we create a "needs" under run build , it means that "run" job won't start until "build" job is done.

- runs-on: In each job we need to define a "Runner" , a Runner is a github hosted virtual machines to run workflows , each Runner needs an image to run on. 
In this case we choose ubuntu-latest.
- steps: Steps section is where you actually use "Actions" of github community or you can create your own action , Here we are using a built in action called "actions/checkout@v2" to simply checkout our git repository to our OS env (ubuntu-latest).
- We need to define a "name" for the step and we also define a "run" to be able run shell commands within our environment.

Your main.yaml file should look like this :

```
name: Python application

on: push
env:
  USER:  "${{secrets.USER}}"
  PASSWD: "${{secrets.PASSWD}}"

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Build docker image.
      run: |
        docker build -f Dockerfile -t "${USER}"/tikal-yahav:1.0.0 .
        docker login --username="${USER}" --password="${PASSWD}"
        docker push "${USER}"/tikal-yahav:1.0.0
        
  run:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Pull docker image and run the container
        run: |
          docker login --username="${USER}" --password="${PASSWD}"
          docker run --name tikal-test -d -p 8080:8080 "${USER}"/tikal-yahav:1.0.0
          
          # Test the container ip and hostname.
          docker ps 
          docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' tikal-test
          docker inspect -f '{{ .Config.Hostname }}' tikal-test 

  
```
### 2. When you done writing main.yaml , as we said earlier - make a push and the workflow will be executed.
- Go to Actions to watch your workflow for detailed information.
- If the build run successfully he will be marked with green.





