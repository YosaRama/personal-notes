---
description: Explain what is github actions and how it's works
---

# Github Actions

## Problem

how do we doing some action automatically for deployment after doing push into Github/Repo?

## Approach

There are a lot of tools for CI/CD. to learn more about CI/CD [click here](continuous-improvement-continuous-delivery-ci-cd.md). Github Actions is one of famous CI/CD tools which is we can used some **custom actions** or **public actions** to doing some action. we just need to create **Configuration File** to define what the actions we need after push the commit into the repo.

**Runner** in Github Actions is one part to running the specific jobs (e.g Linux VM, Windows VM, MacOS VM), there are some **Github Hosted Runner** check [here](https://docs.github.com/en/actions/using-jobs/choosing-the-runner-for-a-job#choosing-github-hosted-runners) for the details and [here how to use it](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners). We also can create our custom **self-hosted runner** with some specific rules from Github kindly check [here](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners) for more information.

**Actions** in Github Actions is one part to doing specific action inside step. there are a lot of actions available on the [marketplace](https://github.com/marketplace?type=actions) created by community and also we can create actions by our self. check [here](https://docs.github.com/en/actions/creating-actions/about-custom-actions) to create custom actions.

## Implementation

Create `.github/workflows` on the repository, then create `github-actions-demo.yml`

```yaml
name: Give a good name for your actions

# Define when you need to run this actions
on:
  push: 
    branches:
      - master

jobs:
  # Define name for your jobs
  print_hello_world:
    # Define where you want to running all of this jobs (can be hosted runners or custom runner)
    runs-on: windows-latest

    steps:
      # Define what action/step we need doing inside the runner, uses = actions (we can find on marketplace or create custom actions)
      - uses: actions/checkout@v4.1.1
      - name: print hello world
        shell: bash
        run: echo "Hello World!"
```

after that push your code to specific branch you mention in this file, viola! your actions will automatically running on your Github Repo

## Q\&A

_**Question: What is Github Actions?**_

Answer: Github action is CI/CD tools to running some actions with specific Images (VM/Docker)

_**Question: How we can get specific actions?**_

Answer: Github have marketplace for public community post custom actions they created, kindly check here [https://github.com/marketplace](https://github.com/marketplace)

_**Question: How specific actions work on Github Actions?**_

Answer: We can create custom actions by **Docker** ([click here](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action)), create a custom actions with **Javascript** ([click here](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action)), or can create custom action with **composite** ([click here](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action))

_**Question: How to store secret information for Github Actions? (e.g Authorization Token)**_

Answer: We need to use Github Secret Action, we can found it on Settings

_**Question: How to access the Github secret action?**_

Answer: We can access Github secret action with `${{secrets.NAME_OF_SECRET}}`

_**Question: Can we have more than 1 workflows?**_

Answer: Yes, just create 2 or more file inside `.github/workflows`

_**Question: How connection between uses in action with name of the jobs?**_

Answer: Simple way to say, uses is the process of installation / bundle process doing something. like `actions/setup-node@v4` that's means the process for this uses is, it will be automatically install node on this **runners** and register node into the PATH

_**Question: when I want to connect between workflow, let's say after workflow 1 done I want to running workflow 2. how can I do that?**_

Answer: inside `on` you can use `workflow_run` and specify which `workflows` and what the `type`

```yaml
name: workflow_2

# Only trigger, when the build workflow succeeded
on:
  workflow_run:
    workflows: ["workflows_1"]
    types:
      - completed
```

_**Question: how to define the actions need to separated inside 1 workflow (with jobs) or we need to create a separated workflows?**_

Answer: of course that will depend on the conditions, if we need running flow/pipelines with different environment, and different trigger action that's need separated workflows. it will be beneficial when every single pipelines will isolated/independent

_**Question: can we call other workflows inside the another workflows?**_

Answer: Yes, it's called reusable workflows. [refer here](https://docs.github.com/en/actions/using-workflows/reusing-workflows#using-inputs-and-secrets-in-a-reusable-workflow)&#x20;

_**Question: then how can we pass secrets from workflows to reuseable workflow?**_

Answer: by `secrets:inherit`
