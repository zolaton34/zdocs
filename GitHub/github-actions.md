# GitHub Actions
## Introduction to GitHub Actions
### Overview
* Event =>
  * Job 1
    * Action 1.1
    * Action 1.2
    * Action 1.3
  * Job 2
    * Action 2.1
    * Action 2.2
    * Action 2.3

* Job 1 =>
  * Runner 1
    * Run actions 1.x
    * Log results
* Job 2 =>
  * Runner 2
    * Run actions 2.x
    * Log results

### The components of GitHub Actions
#### Workflows
* Made up one or more jobs
* can be either
  * scheduled, or
  * triggered by an event
Used to build, test, package, release, or deploy a project on GitHub.

#### Events
* specific activity that triggers a workflow
* can be set to include or exclude branches, tags or paths
* examples
  * push event
  * pull request event

#### Jobs
* set of steps that execute on the same runner
* by default multiple jobs run in parallel
* but can be set to run sequentially (usufull if there is a dependency between jobs)

#### Steps
* individual task that run commands in a job
* can be either
  * an action or
  * a shell command
* each step in a job executes on the same runner and share the same data

#### Actions
* standalone commands
* combined into steps to create a job
* smallest building block of a workflow
* can be custom made or created by the GitHub community
* to use it, include it as a step in a workflow

#### Runners
* a server that has the GitHub Actions runner application installed
* hosted by GitHub, or
* custom hosted your own
* listens for available jobs, runs one job at a time, and reports the progress, logs, and results back to GitHub

### Create an example workflow
* uses YAML syntax
* stored in a directory called .github/workflows
  * in the code repository
  * special constraint: workflow_dispatch (manually) triggered must be stored in the default branch
```yaml
# Optional - The name of the workflow as it will appear in the Actions tab of the GitHub repository.
name: learn-github-actions
# Triggering event
on: [push]
# groups all the jobs in the workflow
jobs:
  # check-bats-version job
  check-bats-version:
    # configure runner
    runs-on: ubuntu-latest
    # groups all steps in check-bats-version job
    steps:
      # uses keyword to retrieve the community action actions/checkout@v2
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      # input values for actions/setup-node@v2
        with:
          # input value for parameter node-version of actions/setup-node@v2
          node-version: '14'
      # runs a command
      - run: npm install -g bats
      - run: bats -v
```

## Finding and customizing actions
### Overview
Action can be defined in:
* A public repository
* The same repository where it is used
* A published Docker container image on Docker Hub
* [GitHub Marketplace](https://github.com/marketplace?type=actions)

### Browsing Marketplace actions in the workflow editor
You can [search and browse actions directly in your repository's](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#browsing-marketplace-actions-in-the-workflow-editor) workflow editor.

### Adding an action to your workflow [see docs](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#adding-an-action-to-your-workflow)
In you workflow editor 
* Navigate to the action you want to use in your workflow.
* Under "Installation", copy the workflow syntax.

### [Using release management for your custom actions](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#using-release-management-for-your-custom-actions)
Community action have the option to use tags, branches, or SHA values to manage releases.
#### Using tags
* Can decide changes between minor and major versions
* Not reliable
```yaml
steps:
  - uses: actions/javascript-action@v1.0.1
```

#### Using SHAs
* The most reliable
* Not automatically updated
* Must full length sha
```yaml
steps:
  - uses: actions/javascript-action@172239021f7ba04fe7327647b213799853a9eb89
```

#### Using branches
* Always updated to the HEAD of the branch
* Not reliable
```yaml
steps:
  - uses: actions/javascript-action@main
```

### [Using inputs and outputs with an action](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)
* Example of [input source code](https://github.com/Zolaton/zdocs/blob/25b1b1fb31bfb9a787d6c78715b2dff488d1fd0a/.github/workflows/io.yml#L11)
* Example of [output source code](https://github.com/Zolaton/zdocs/blob/25b1b1fb31bfb9a787d6c78715b2dff488d1fd0a/.github/workflows/io.yml#L48)

### [Referencing an action in the same repository where a workflow file uses the action](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#referencing-an-action-in-the-same-repository-where-a-workflow-file-uses-the-action)
* Can be referenced with
  * `{owner}/{repo}@{ref}`. In that case the `action.yaml` file must be at the root of the referenced `{owner}/{repo}@{ref}`
  * `./path/to/dir`. In that case we use the repository file structure shown below.
#### Example repository file structure
```
|-- hello-world (repository)
|   |__ .github
|       |__ workflows
|           |__ my-first-workflow.yml
|       |__ actions
|           |__ hello-world-action
|               |__ action.yml
```

### [Referencing a container on Docker Hub](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#referencing-a-container-on-docker-hub)
* Referenced with `docker://{image}:{tag}`
* Code example of [building a Docker image for use in an action and how to it](https://github.com/Zolaton/hello-world-docker-action#hello-world-docker-action).
##### Example
```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: docker://alpine:3.8
```

## [Essential features of GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions)

### [Using variables in your workflows](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions#using-variables-in-your-workflows)
* Actions include [default environment variables](https://docs.github.com/en/actions/reference/environment-variables#default-environment-variables)
* We can also set custom environment variables like below
```
jobs:
  example-job:
      steps:
        - name: Connect to PostgreSQL
          run: node client.js
          env:
            POSTGRES_HOST: postgres
            POSTGRES_PORT: 5432
```

### [Adding scripts to your workflow](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions#adding-scripts-to-your-workflow)
Example:
```yaml
jobs:
  example-job:
    steps:
      - name: Run build script
        run: ./.github/scripts/build.sh
        shell: bash
```

### [Sharing data between jobs](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions#sharing-data-between-jobs)
Artifacts are used
* to share files between jobs in the same workflow
* store these files in GitHub. They will be accessible 
  * [From the GitHub UI](https://docs.github.com/en/actions/managing-workflow-runs/downloading-workflow-artifacts)
  * [With the GitHub CLI](https://docs.github.com/en/actions/managing-workflow-runs/downloading-workflow-artifacts)
  * [With the REST API](https://docs.github.com/en/rest/reference/actions#artifacts)

Example of [code source using sharing with artifacts](https://github.com/Zolaton/zdocs/blob/515e0887c62b3f80ab92552fd6e65e896391379b/.github/workflows/sharing.yml#L6)

## [Managing complex workflows](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows)

### [Storing secrets](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#storing-secrets)
* Encryted secrets can be stored in
  * an organization
  * a repository
  * a repository environment

#### Naming your secrets
* See [Naming your secrets](https://docs.github.com/en/actions/reference/encrypted-secrets#naming-your-secrets)

#### Secret reference
* [Code example](https://github.com/Zolaton/zdocs/blob/36fabba4fd4fe006ed4387be99afe12d0cd22ca4/.github/workflows/secrets.yml#L5)
* [Encrypted secrets](https://docs.github.com/en/actions/reference/encrypted-secrets)

### [Creating dependent jobs](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#creating-dependent-jobs)
Example
```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - run: ./setup_server.sh
  build:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - run: ./build_server.sh
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./test_server.sh
```
More info see [jobs.<job_id>.needs](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds).

### [Using a build matrix](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#using-a-build-matrix)
Example
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [6, 8, 10]
    steps:
      - uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node }}
```
* [Matrix code example](https://github.com/Zolaton/zdocs/blob/7001cc93b1877e24e42052890b5f65a1f34f2d52/.github/workflows/matrix.yml#L5)

### [Caching dependencies](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#caching-dependencies)
* When cache miss
  * Lookup restore-keys for partial match and restore the most recent if found any
  * At the end, saves the new cache content, which includes
    * The restored cached content, if any
    * Overrided by the new content in the cache path
* When cache hit
  * Restore cache content
  * At the end, does not save any cache content

#### Cachin reference
* [Action cache](https://github.com/actions/cache#cache)
* [Caching dependencies to speed up workflows](https://docs.github.com/en/actions/guides/caching-dependencies-to-speed-up-workflows)
* [Code example](https://github.com/Zolaton/zdocs/blob/73a432567c920ac16da52a8bbd510cb0cd61962b/.github/workflows/cache.yml#L5)

### [Using databases and service containers](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#using-databases-and-service-containers)
* For job that requires a database or cache service, use the `services` keyword.
* It creates an container to host the service
* Available until the job has completed.

#### [Example 1 - Creating Redis service containers](https://docs.github.com/en/actions/guides/creating-redis-service-containers#testing-the-redis-service-container)
* [Code source](https://github.com/Zolaton/zdocs/blob/b02ad1a60ab7a4653ca782d61fb4bd64a1e7335e/.github/workflows/redis-service-container.yml#L4)

### Service reference
* [About service containers](https://docs.github.com/en/actions/guides/about-service-containers)

### [Using labels to route workflows](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#using-labels-to-route-workflows)
TODO

### [Using environments](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#using-environments)
* Environments define protection rules and secrets
* Each job in a workflow can reference a single environment.
* Any protection rules for the environment must pass 
  * before the job referencing it is sent to a runner
  * before the job can access its secrets
* Only available for GitHub public repos or GitHub Enterprise repos

#### [Environment protection rules](https://docs.github.com/en/actions/reference/environments#environment-protection-rules)
##### [Required reviewers](https://docs.github.com/en/actions/reference/environments#required-reviewers).
* Can list up to 6 reviewers or teams
* Reviewers must have at least read access to the repo
* Only one approval is required
* If a job is not approved within 30 days, the workflow run will be automatically canceled
* See also [Reviewing deployments](https://docs.github.com/en/actions/managing-workflow-runs/reviewing-deployments)
##### [Wait timer](https://docs.github.com/en/actions/reference/environments#wait-timer)
* Integer between 0 and 43,200 (30 days).
##### [Deployment branches](https://docs.github.com/en/actions/reference/environments#deployment-branches)

#### [Using a workflow template](https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows#using-a-workflow-template)
* See list [available templates](https://github.com/actions/starter-workflows)



## References
* [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)
* [GitHub Community Support's GitHub Actions category](https://github.community/c/code-to-cloud/github-actions/41)

## TODO
* Offical GitHub documentation
  * [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idsteps)
  * [Using inputs and outputs with an action](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)
  * [Keeping your actions up to date with Dependabot](https://docs.github.com/en/github/administering-a-repository/keeping-your-actions-up-to-date-with-dependabot)
* Other
  * [4 Steps to Creating a Custom GitHub Action](https://betterprogramming.pub/4-steps-to-creating-a-custom-github-action-d67c4cf0445a)