# [Authentication in a workflow](https://docs.github.com/en/actions/reference/authentication-in-a-workflow)
## [About the `GITHUB_TOKEN` secret](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#about-the-github_token-secret)
* `GITHUB_TOKEN` is automatically created when running the workflow.
* Token's permissions are limited to the workflow repository.
* For each job
  * Token is created before the job starts
  * Token expires when the job is finished
* Token also available in the github.token context. See [Context and expression syntax for GitHub Actions](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions#github-context)

## [Using the `GITHUB_TOKEN` in a workflow](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#using-the-github_token-in-a-workflow)
* Can be referenced with `${{ secrets.GITHUB_TOKEN }}`

## [Permissions for the GITHUB_TOKEN](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#permissions-for-the-github_token)
* See [permissions table](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#permissions-for-the-github_token)
* Default access types
  * Can be changed in the enterprise, organization or repository settings
    * Permissive
    * Restrictive
  * For forked repositories maximum access is read
### [Modifying the permissions for the `GITHUB_TOKEN`](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#modifying-the-permissions-for-the-github_token)
* Permissions can be changed in individual workflow files
* When the `permissions` key is used, all unspecified permissions are set to no access, `metadata`, which is always read access
* By default, write access cannot be added on forked repositories (except if it has the `Send write tokens to workflows from pull requests` setting)
### [How the permissions are calculated for a workflow job](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#how-the-permissions-are-calculated-for-a-workflow-job)
* Gets the most restrictive default in its parents (repos, organization, enterprise)
* Then apply settings in workflow file
  * At the workflow level
  * Then at the job level
* If event is `pull_request` and `Send write tokens to workflows from pull requests` setting is not enable permmissions are set to read only
### [Granting additional permissions](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#granting-additional-permissions)
If you need a token that requires permissions that aren't available in the `GITHUB_TOKEN`, you can create a personal access token and set it as a secret in your repository.