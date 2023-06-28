# Week 1 - Project 

# Goal
To Establish a Robust and Reliable Development process by settting version control using Git, implementing comprehensive Unit Tests, and setting up effective workflows to ensure that only properly tested code is merged from other branches into the Master branch.

## Requirements

<br> Link to setup Python 3.9 - https://www.python.org/downloads/ </br>
<br> Link to setup Git the first time - https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup </br>
<br> Use a Editor of your choice </br>
<br> Link to get pip3 - https://pip.pypa.io/en/stable/installation/ </br>

## Step 1 - Get the inital start code onto your system

Git clone initial code from this Github Repo - https://github.com/vj-m/week1-devops/tree/initial_starter_code
```
git clone git@github.com:vj-m/week1-devops.git
```
## Step 2 - Add Unit Tests

```
def test_add():
    assert add(1,1) == 2

def test_sub():
    assert sub(1,1) == 0

def test_mul():
    assert mul(1,1) == 1

def test_div():
    assert div(2,1) == 2
```

## Step 3 - Create a workflow

1.) Create a directory called .github/workflows within the directory containing .git
2.) And create a workflow ‘YAML’ file, let's call it ci_cd_wf.yml
3.) Git add and push changes (This automatically invokes the Git actions and sets up the CI/CD Pipeline which on push to the master branch)

```
name : test

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on : ubuntu-latest

    steps:
      - name: Check out repo code
        uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v3
        with:
          python-version: "3.x"

      - name: Install Dependencies
        run: |
          python -m pip install -r requirements.txt
      - name: Run tests
        run: |
          python -m pytest calculator.py
```

## Step 4 - Check our Gitflow with a new branch
<br> 1.)  Create a new feature branch to check a case where the push to master or PR fails by making a purposeful mistake in the code to fail a test </br>
<br> 2.)  Create a new feature branch called test </br>

```
def test_add():
    assert add(1,1) == 0
```
<br> We have changed the test_add method to assert a False </br>
<br> 3.) Git add, push and commit </br>

## Step 5 - Fix all the errors to make a successful build
<br> 1.) Change back the test_add function </br>
<br> 2.) Add, commit, and push the new changes to the master branch again </br>

## Optional
<br> 1.) Added pytest-cov to generate</br>
The edited workflow `YAML` generates an `XML` coverage report using pytest-cov. You can access and download the report from the GitHub Actions workflow run page under the "coverage-report" artifact. For more interactive and graphical visualization, additional tools or online services can be used with the generated XML report.

`.github/workflows/ci-cd.yml`:
```
name: test

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - "*"

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repo code
        uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v3
        with:
          python-version: "3.x"

      - name: Install Dependencies
        run: |
          python -m pip install -r requirements.txt

      - name: Run tests
        run: |
          python -m pytest -v tests

      - name: Install pytest-cov
        run: |
          python -m pip install pytest-cov

      - name: Generate coverage report
        run: |
          python -m pytest --cov=calculator --cov-report=xml:cov.xml

      - name: Upload coverage report
        uses: actions/upload-artifact@v2
        with:
          name: coverage-report
          path: cov.xml
```
