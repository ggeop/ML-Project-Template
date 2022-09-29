# ml-project-template

This project contains a blueprint of a production ML structure.
Also can guide a user how to organize the code to follow a CI/CD flow.
The structure is designed to support a project with multiple data sources, models and ML pipelines.

# Target Audience :loudspeaker:
* Data Engineers
* Data Scientists
* Data Analysts
* Software Engineers
* MLOps Engineers
* Machine Learning (ML) Engineers
# Introduction
In production based applications the automation in building, testing and deployment is crusial.
The less automated the project lifecycle is the more time consuming and error prone is.
For this reason, the modern CI/CD practices are more relevant than ever.

So, before go into project structure details let's give a look of a CI/CD.

# Suggested Technology Stack
* **Running Environment**: Docker/AKS
* **Programming language**: Python
* **Python Virtual Environment**: Pyenv/Conda

***NOTE:** The blueprint example is based on Python but can be generallized in every modern object oriented language*
# CI/CD Flow Suggestion
The first thing before start describing the CI/CD pipelines, is to establish the
CI/CD strategy of the project. Also, it's worth to mention that production based applications should have different enviroments for testing defore the final deployment.
For that purpose, we are suggesting to use the following environments:
* **Development (not included in the diagrams)**: Development environment is used only for development team for testing new features before adapt other environments or open a PR
* **Integration**: Used for automated integration tests (no manual interactions)
* **Staging**: A replica of Production environment and a full environment to interact.
Usually it's only open to development team and key stake holders for their testing. Also, it's open for user/stakeholder acceptance testing
* **Production**: Open to all who have access.

![alt text](photos/cicd_flow.png)

***NOTE:** The below CI/CD flow is also a blueprint. Each project has specific needs and compliance policies which has to follow*


  * **Continues Integration (CI)**
    * In Feature branch
      * Build Docker image
      * Run unittests (in Docker image)
      * Run other checks, e.g SonarCube, Snik (in Docker image)
      * Merge feature_branch to develop
    * In Develop branch (Runs in: Integration env)
      * Create a Release Docker image
      * Run integration tests
      * Merge develop to master
    * In Master branch
      * Tag the latest commit with `Staging`
      * Deploy the new release Docker image in Staging env
      * Acceptance test
    * **Continues Delivery (CD)**
      * Add a tag `Production` in the `Staging` commit
      * Deploy the release Docker image in Production env


# Structure

In high level the main components of a ML production project are:

```
|-- cicd        <-- CI/CD (packaging, scripts & configurations)
|-- notebooks   <-- Explanatory analysis & new models POC
|-- tests       <-- Code testing (unit & integration)
|-- src         <-- Source code for production use
```

The full structure of the project is:
```
|-- LICENCE
|-- README.md
|-- .gitignore
|-- notebooks
|   |-- dummy_data
|   |-- model_poc.ipynb
|-- cicd/
|   |-- README.md
|   |-- build/
|   |   |-- utils/
|   |   |-- docker/
|   |   |  |-- .dockerignore
|   |   |  |-- Dockerfile
|   |   |-- build.sh
|   |-- release/
|       |-- docker/
|       |   |-- .dockerignore
|       |   |-- Dockerfile
|       |-- release.sh
|-- src/
    |-- MANIFEST.in
    |-- pyproject.toml
    |-- setup.py
    |-- requirements/
    |   |-- default.txt
    |   |-- dev.txt
    |-- tests/
    |   |-- __init__.py
    |   |-- .coveragerc
    |   |-- unit/
    |   |-- integration/
    |   |-- run_tests.py
    |-- <PYTHON_PACKAGE_NAME>
        |-- __init__.py
        |-- data_loaders/
        |   |-- data_loader.py
        |   |-- csv_data_loader.py
        |   |-- ...
        |-- models/
        |   |-- common/
        |   |   |-- __init__.py
        |   |   |-- ...
        |   |-- rule_based/
        |   |   |-- __init__.py
        |   |   |-- naive_model.py
        |   |   |-- ...
        |   |-- regression/
        |   |   |-- __init__.py
        |   |   |-- olsr_regressor.py
        |   |   |-- ...
        |   |-- ensemble/
        |       |-- __init__.py
        |       |-- random_forests/
        |       |   |-- __init__.py
        |       |   | -- no_features.py
        |       |   | -- all_features.py
        |       |   |-- ...
        |       |-- boosting/
        |       |   |-- __init__.py
        |       |   |-- ...
        |       |-- ...
        |-- features/
        |   |-- __init__.py
        |   |-- common/
        |   |   |-- __init__.py
        |   |-- build_feature_one/
        |   |   |-- __init__.py
        |   |   |-- prefilter.py
        |   |   |-- filter.py
        |   |   |-- portfilter.py
        |   |   |-- processor.py
        |   |-- build_features_two/
        |       |-- __init__.py
        |       |-- tokenizer.py
        |-- pipelines/
        |   |-- train.py
        |   |-- predict.py
        |-- run.py
```