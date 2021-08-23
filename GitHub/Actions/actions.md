# [Creating Actions](https://docs.github.com/en/actions/creating-actions)
Types (see details down the page):
* Docker container actions
* JavaScript actions
* Composite run steps actions

## Recommendations
### [Choosing a location for your action](https://docs.github.com/en/actions/creating-actions/about-actions#choosing-a-location-for-your-action)
* In its own repository if shared
* If not shared recommandation is to put them under `.github/actions`

### [Compatibility with GitHub Enterprise Server](https://docs.github.com/en/actions/creating-actions/about-actions#compatibility-with-github-enterprise-server)
For compatibility with GitHub Enterprise Server do not hard code references to GitHub API URLs. Instead use environment variables:
* `GITHUB_API_URL` for REST API
* `GITHUB_GRAPHQL_URL` for GraphQL

### [Using release management for actions](https://docs.github.com/en/actions/creating-actions/about-actions#using-release-management-for-actions)
#### [Good practices for release management](https://docs.github.com/en/actions/creating-actions/about-actions#good-practices-for-release-management)
* Use some release management
* Major versions should contain critical fixes and security patches
* Default branch should not be referenced by users. Instead use one of the following
  * Commit SHA
  * Tag
  * Branch named for a release

#### [Using tags for release management](https://docs.github.com/en/actions/creating-actions/about-actions#using-tags-for-release-management)
We can easily distinguish between major and minor versions.
* Use a release branch such as `release/v1` before creating the release tag (for example, `v1.0.2`)
* [Create a release](https://docs.github.com/en/articles/creating-releases) using semantic versioning
* Move the major version tag (`v1`) to point to the current release
* Introduce a new major version tag (`v2`) for changes that will break existing workflows
* Major versions can be initially released with a `beta` tag to indicate their status, for example, `v2-beta`

#### [Using branches for release management](https://docs.github.com/en/actions/creating-actions/about-actions#using-branches-for-release-management)

#### [Using a commit's SHA for release management](https://docs.github.com/en/actions/creating-actions/about-actions#using-a-commits-sha-for-release-management)

#### [Creating a README file for your action](https://docs.github.com/en/actions/creating-actions/about-actions#creating-a-readme-file-for-your-action)
* Detailed description of what the action does
* Required input and output arguments
* Optional input and output arguments
* Secrets the action uses
* Environment variables the action uses
* An example of how to use your action in a workflow

## [Docker container actions](https://docs.github.com/en/actions/creating-actions/about-actions#docker-container-actions)
* For better consistency and reliability
* Slower than other actions

### [Example of docker action](https://github.com/Zolaton/hello-world-docker-action)
#### Docker container action code
* [Metadata file (action.yaml)](https://github.com/Zolaton/hello-world-docker-action/blob/5177c37e8a48ba85369842df0d8ae269274c8a01/action.yaml#L1)
* [README](https://github.com/Zolaton/hello-world-docker-action#hello-world-docker-action)
* [Dockerfile](https://github.com/Zolaton/hello-world-docker-action/blob/5177c37e8a48ba85369842df0d8ae269274c8a01/Dockerfile#L1)
* [entrypoint.sh](https://github.com/Zolaton/hello-world-docker-action/blob/5177c37e8a48ba85369842df0d8ae269274c8a01/entrypoint.sh#L1)
  * This is container entrypoint
  * See in particular example of how to set an output in a bash script: `echo "::set-output name=time::$time"`

#### Docker container action usage
  See [workflow](https://github.com/Zolaton/hello-world-docker-action/blob/fa2e217870a471382cd74d9417cc97a1852d89e9/.github/workflows/main.yaml#L1)

## [JavaScript actions](https://docs.github.com/en/actions/creating-actions/about-actions#javascript-actions)
* Run directly on the runner
* Must be pure Javascript (no other binaries)

### [Example of javascript action](https://github.com/Zolaton/hello-world-javascript-action)
#### Javascript action code
Take note we initialized the project for node, including the required modules with the following commands
```bash
npm init -y
npm install @actions/core
npm install @actions/github
```

* [Metadata file (action.yml)](https://github.com/Zolaton/hello-world-javascript-action/blob/058f78e45f628fdabaa52d073374251df5c0c4e5/action.yml#L1)
* [README](https://github.com/Zolaton/hello-world-javascript-action/blob/main/README.md#hello-world-javascript-action)
* [index.js (actual action code)](https://github.com/Zolaton/hello-world-javascript-action/blob/058f78e45f628fdabaa52d073374251df5c0c4e5/index.js#L1)

#### Javascript usage


## [Composite run steps actions](https://docs.github.com/en/actions/creating-actions/about-actions#composite-run-steps-actions)
* Bundle different step into a single action. For example bundle run commands as a single step.
