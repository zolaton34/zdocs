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
  * special constraint: workspace_dispatch (manually) triggered must be stored in the default branch
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
# Browsing Marketplace actions in the workflow editor
You can [search and browse actions directly in your repository's](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#browsing-marketplace-actions-in-the-workflow-editor) workflow editor.
# [Adding an action to your workflow](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#adding-an-action-to-your-workflow)
In you workflow editor 
* Navigate to the action you want to use in your workflow.
* Under "Installation", copy the workflow syntax.
## References
* [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)
* [GitHub Community Support's GitHub Actions category](https://github.community/c/code-to-cloud/github-actions/41)
## TODO
* [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idsteps)
* [Using inputs and outputs with an action](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)
* [Keeping your actions up to date with Dependabot](https://docs.github.com/en/github/administering-a-repository/keeping-your-actions-up-to-date-with-dependabot)