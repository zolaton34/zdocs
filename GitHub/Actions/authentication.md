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