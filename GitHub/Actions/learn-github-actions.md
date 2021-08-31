# [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)
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

#### Caching reference
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

## [Sharing workflows with your organization](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization)

### [Creating a workflow template](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization#creating-a-workflow-template)
* Worflow templates are created in the .github repository of the organization
* Can be used by organization members with permission to create workflows

#### Procedure to create workflow template
1. Create a public repository named `.github` in the organization
2. Create a `workflow-templates` directory
3. Create the workflow file inside the `workflow-templates` directory. Example [`octo-organization-ci.yml`](https://github.com/Zolaton/.github/blob/20f35e76f126351d79d2e12fe1f30ad3baada15a/workflow-templates/octo-organization-ci.yml#L1).
```yaml
name: Octo Organization CI
on:
  push:
    branches: [ $default-branch ]
  pull_request:
    branches: [ $default-branch ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Run a one-line script
        run: echo Hello from Octo Organization
```
4. Create a metadata file inside the `workflow-templates`. Example [`octo-organization-ci.properties.json`](https://github.com/Zolaton/.github/blob/20f35e76f126351d79d2e12fe1f30ad3baada15a/workflow-templates/octo-organization-ci.properties.json#L1).
```json
{
    "name": "Octo Organization Workflow",
    "description": "Octo Organization CI workflow template.",
    "iconName": "workflow-icon",
    "categories": [
        "Go"
    ],
    "filePatterns": [
        "package.json$",
        "^Dockerfile",
        ".*\\.md$"
    ]
}
```

### [Using a workflow template from your organization](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization#using-a-workflow-template-from-your-organization)

### [Sharing secrets within an organization](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization#sharing-secrets-within-an-organization)

### [Share self-hosted runners within an organization](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization#share-self-hosted-runners-within-an-organization)
TODO

## [Security hardening for GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions)
### [Using secrets](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-secrets)
* Never use structured data as a secret
* Register all secrets used within workflows included sensitive information created by the workflow and secret transformed with the workflow (for example base64).
* Audit how secrets are handled
  * Review the source code of the repository executing the workflow
  * Check any actions used in the workflow
* View the run logs for your workflow after testing valid/invalid inputs
* Use credentials with minimal scope
* Restrict write access to your repository
* Set `GITHUB_TOKEN` permission to `read-only`. [Increase its privileges](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#modifying-the-permissions-for-the-github_token) only if strictly needed and a very restricted envinromnent / workflow
* Audit and rotate registered secrets
* Audit and rotate secrets
* [Require reviewers](https://docs.github.com/en/actions/reference/environments#required-reviewers) to protect environment secrets

### [Use CODEOWNERS to monitor changes](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-codeowners-to-monitor-changes)
For example `.github/workflow` to monitore changes in the workflows.

### [Understanding the risk of script injections](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#understanding-the-risk-of-script-injections)
* Always consider your code might execute untrusted input from attackers
* Attackers can add their own malicious content to the github context, which should be treated as potentially untrusted input

#### [Example of a script injection attack](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#understanding-the-risk-of-script-injections)
See [code source](https://github.com/Zolaton/zdocs/blob/8fc441aae86be41fd83a103da9e278525e230c38/.github/workflows/scrinj.yml#L2)

### [Good practices for mitigating script injection attacks](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#good-practices-for-mitigating-script-injection-attacks)

#### [Using an action instead of an inline script (recommended)](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-an-action-instead-of-an-inline-script-recommended)
* Create an action that processes the context value as an argument
* Example [code](https://github.com/Zolaton/zdocs/blob/2488d85cdea5222bada940ecd81d8208a48e39ce/.github/workflows/mitig1.yml#L1):
```yaml
    steps:
      - uses: actions/checkout@v2
      - uses: ./.github/actions/mitigation-action
        with:
          your-name: ${{ github.event.inputs.your-name }}
```
#### [Using an intermediate environment variable](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-an-intermediate-environment-variable)
* Prefered for inline scripts
* Example
```yml
      - name: Second mitigation
        env:
          YOUR_NAME: ${{ github.event.inputs.your-name }}
        run: echo $YOUR_NAME
```

#### [Using CodeQL to analyze your code](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-codeql-to-analyze-your-code)
* TODO =  better understanding

#### [Restricting permissions for tokens](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#restricting-permissions-for-tokens)
* See also [Modifying the permissions for the GITHUB_TOKEN](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#restricting-permissions-for-tokens)

### [Using third-party actions](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-third-party-actions)
Mitigate risks of running third-party actions
* Pin actions to a full length commit SHA
  * Only way to use an immutable release of an action
* Audit the source code of the action
* Pin actions to a tag only if you trust the creator

### [Potential impact of a compromised runner](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#potential-impact-of-a-compromised-runner)
* [Accessing secrets](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#accessing-secrets)
* [Exfiltrating data from a runner](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#exfiltrating-data-from-a-runner)
* [Stealing the job's `GITHUB_TOKEN`](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#stealing-the-jobs-github_token)
* [Modifying the contents of a repository](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#modifying-the-contents-of-a-repository)
Recommended approaches for accessing repository data within a workflow, in descending order of preference:
1. The `GITHUB_TOKEN`
2. Repository deploy key
  * These are SSH keys stored at the repository level
  * Can only be used for pull and (optionally) push git like commands
3. Personal access tokens
  * DO NOT USE IT
4. SSH keys on a user account
  * DO NOT USE IT

### [Considering cross-repository access](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#considering-cross-repository-access)
Recommended approaches for accessing repository data within a workflow, in descending order of preference
1. `GITHUB_TOKEN`
2. Repository deploy key
  * One of the only credential types that grant read or write access to a single repository
  * Can be used to interact with another repository
  * Can only clone or push to the git repository
  * Cannot give access to the REST or GraphQL API
  * see [Managing deploy keys](https://docs.github.com/en/developers/overview/managing-deploy-keys#deploy-keys)

### [Hardening for self-hosted runners](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#hardening-for-self-hosted-runners)
* Prefer _GitHub-hosted runners_ that are ephemeral virtual machine
* _Self-hosted_ runners on GitHub can be persistently compromised by untrusted code in a workflow
  * Should almost never be used for public repositories
  * Be also cautious when using self-hosted runners on private repositories
  * We ca reduce the scope of a compromise, by organizing self-hosted runners into separate groups
  * Connsider also the environment of the self-hosted runner machines:
    * Private SSH Keys, API tokens etc.
    * The amount of sensitive information in this environment should be kept to a minimum

### [Auditing GitHub Actions events](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#auditing-github-actions-events)
GitHub Actions events in the audit log
* [Events for environments](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-environments)
* [Events for configuration changes](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-configuration-changes)
* [Events for secret management](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-secret-management)
* [Events for self-hosted runners](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-self-hosted-runners)
* [Events for self-hosted runner groups](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-self-hosted-runner-groups)
* [Events for workflow activities](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#events-for-workflow-activities)

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