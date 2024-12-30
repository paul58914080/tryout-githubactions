# Tryout Github Actions

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

### As map with multiple events and paths
```yaml
on:
  push:
    branches:
      - main
      - master
    paths:
      - 'docs/**'
      - 'test/**'
```

### As map with multiple events and paths and tags
```yaml
on:
  push:
    branches:
      - main
      - master
    paths:
      - 'docs/**'
      - 'test/**'
    tags:
      - v1
      - v1.*
```

### As map with multiple events and paths and tags and condition
```yaml
on:
  push:
    branches:
      - main
      - master
    paths:
      - 'docs/**'
      - 'test/**'
    tags:
      - v1
      - v1.*
    condition: ${{ github.event_name == 'pull_request' }}
```


### As map with multiple events and paths and tags and condition and types
```yaml
on:
  push:
    branches:
      - main
      - master
    paths:
      - 'docs/**'
      - 'test/**'
    tags:
      - v1
      - v1.*
    condition: ${{ github.event_name == 'pull_request' }}
    types:
      - created
      - edited
      - reopened
      - synchronize
```