# Introduction

Hi , Here i will show you how to build a CI proccess with Github-Actions , In this senario we have a python app repository that contains all the files needed and we want to implement it in a test environemnt to check if everything is ok.


# Problem

Automate ci proccess of python app with github actions using dokcer.

# Requirements

1. Fork this repository to your github account - https://github.com/lvthillo/python-flask-docker
2. Go to Settings > Actions and make sure "Allow all actions" is checked.
3. In order to github-actions will work we need to create a folder called .github/workflows

# Solution

1. Building a simple workflow named main.yaml
- Name - Is the name of your workflow "Python application" in our case.
- on: push - means on each push action the workflow will execute.
- 

2. Use ubuntu image for the runner
3. Check out the git repository
4. Create a step that will Build the docker image and run it.