# Tryout Github Actions

## Syntax

```yaml
name: [The job name that will be displayed on the Github UI]
on: [The event that triggers the workflow]
jobs:
  [job_id]: # [The job id that will be displayed on the Github UI]  
    name: [The job name that will be displayed on the Github UI]
    runs-on: [The runner that the job will run on]
    steps:
      - name: [The step name that will be displayed on the Github UI]
        uses: [The action that will be used]
        with:
          [key]: [value] # [The input that will be passed to the action]
        run: [The shell command that will be executed]
```

> [Read more about Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)

## Triggers

You would be able to trigger the workflow mainly by the following ways:
- [Repository events](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)
- Manual triggers (`workflow_dispatch`)
  - Through the Github UI
  - Through the Github API
  - Another workflow
- Schedule
  - Cron syntax

There are several ways to declare the event that triggers the workflow.

### As list
```yaml
on: [push, pull_request, workflow_dispatch]
```

### As map
```yaml
on:
  push:
  pull_request:
  workflow_dispatch:
```

### As map with multiple events
```yaml
on:
  push: # event 1
    branches:
      - main
      - master
  pull_request: # event 2
    branches:
      - main
      - master
  workflow_dispatch: # event 3
  schedule: # event 4
    - cron: '0 0 * * *'
```

### As map with branches and paths
```yaml
on:
  push:
    branches:
      - main
      - master
    branches-ignore:
      - dev
    paths:
      - '!docs1/**' #not on docs1
      - 'test/**'
    paths-ignore:
      - 'docs/README.md' # similar to '!docs/README.md' in paths
      - 'test/README.md'
```

### As map with tags
```yaml
on:
  push:
    tags:
      - v1
      - v1.*
    tags-ignore:
      - v1.1
      - v1.2
```

### As map with condition
```yaml
on:
  push:
    condition: ${{ github.event_name == 'pull_request' }}
```


### As map with types
```yaml
on:
  push:
    types:
      - created
      - edited
      - reopened
      - synchronize
```

- When chaining GitHub Actions events like push, pull_request, etc., it is an "or" condition. This means that the workflow will be triggered if any of the specified events occur.
- When you use `branches`/`branches-ignore`, `paths`/`paths-ignore`, `tags`/`tags-ignore`, and `types`, it is an "and" condition. This means that the workflow will be triggered if all the specified conditions are met.

## Runners

There are mainly two types of runners:
- Github-hosted runners: These are virtual machines hosted by Github and are managed services. A VM is created for each job and destroyed after the job is completed. You can use the pre-installed software on these runners or install additional software using the `actions/setup-node` action. You can also use the `actions/cache` action to cache dependencies between jobs. By default, you get 20 concurrent jobs per repository and clean VMs for each job.
- Self-hosted runners: These are machines that you manage and maintain

## Activity types

The official documentation about GitHub Action types can be found on the GitHub Docs website. It provides detailed information on the various events that can trigger workflows, including repository events, manual triggers, and scheduled events.

You can refer to the [GitHub Actions Events that trigger workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows) page for comprehensive details.

## Context

We use the following syntax to access the context in the workflow:

```yaml
${{ github.context }}
```

The `github` context is an object that contains a variety of information about the event that triggered the workflow. You can use this context to access information about the repository, the workflow, the event, and the actor that triggered the event.

The `event` context is an object that contains the payload of the event that triggered the workflow. You can use this context to access information about the event, such as the type of event, the action that triggered the event, and the payload data.

The `inputs` context is an object that contains the input parameters passed to the action. You can use this context to access the input parameters and use them in your workflow.

The `secrets` context is an object that contains the secrets stored in the repository. You can use this context to access the secrets and use them in your workflow.

- [More about contexts](https://docs.github.com/en/enterprise-cloud@latest/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs)
- [Context availability](https://docs.github.com/en/enterprise-cloud@latest/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#context-availability)

If you wish to debug you could use `toJSON` function to print the context:

```yaml
- name: Print context
  run: echo '${{ toJson(github) }}'
```

## Functions

https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/evaluate-expressions-in-workflows-and-actions#functions

| Function name | Description                                                                                                                                      |
|:--------------|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| `success()`   | Returns true if all the previous step have succeeded                                                                                             |
| `always()`    | Causes the step to always run, regardless of the status of previous steps                                                                        |
| `cancelled()` | Returns true if the workflow was cancelled                                                                                                       |
| `failure()`   | Returns true when any previous step of a job fails. If you have a chain of dependent jobs, `failure()` returns `true` if any ancestor job fails. |