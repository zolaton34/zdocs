# GitHub Action Guides

## [Publishing Docker images](https://docs.github.com/en/actions/guides/publishing-docker-images)
* [Publish to DockerHub](https://github.com/Zolaton/zdocs/blob/edf982ff48630f91e103dc22fba8e43456752cb1/.github/workflows/dockerhub-publish.yml#L1)
* [Publish as GitHub package](https://github.com/Zolaton/zdocs/blob/edf982ff48630f91e103dc22fba8e43456752cb1/.github/workflows/docker-github-publish.yml#L1)
* [Publish to DockerHub & as GitHub package](https://github.com/Zolaton/zdocs/blob/edf982ff48630f91e103dc22fba8e43456752cb1/.github/workflows/docker-multi-reg-publish.yml#L1)

## [Deploying to Amazon Elastic Container Service](https://docs.github.com/en/actions/guides/deploying-to-amazon-elastic-container-service)
* TODO

## [Deploying to Azure App Service](https://docs.github.com/en/actions/guides/deploying-to-azure-app-service)
### [Prerequisites](https://docs.github.com/en/actions/guides/deploying-to-azure-app-service#prerequisites)
* Run:
  ```bash
  az login
  # Create resource group
  az group create -l canadaeast -n mikatu
  # Create an Azure App Service plan
  az appservice plan create --resource-group mikatu --name jikalo-service-plan --is-linux
  # Create a web app.
  az webapp create --name jikalo-web --plan jikalo-service-plan --resource-group mikatu --runtime "node|10.14"
  ```
* Configure an Azure publish profile and create an `AZURE_WEBAPP_PUBLISH_PROFILE` secret. (See [Generate deployment credentials](https://docs.microsoft.com/en-us/azure/app-service/deploy-github-actions?tabs=applevel#generate-deployment-credentials) and [Encrypted secrets](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository))
### Deploying to Azure Workflow
* See [workflow code](https://github.com/Zolaton/zdocs/blob/11324953d167ed173abc05b9ef74b84a3567f72c/.github/workflows/azure-publish.yml#L1).

## [Deploying to Google Kubernetes Engine](https://docs.github.com/en/actions/guides/deploying-to-google-kubernetes-engine)
### [Prerequisites](https://docs.github.com/en/actions/guides/deploying-to-google-kubernetes-engine#prerequisites)
* Run
  ```bash
  # Create project
  gcloud container clusters create lozatar-cluster --project=mulqatu-project --zone=us-east4-b

  ```
* TODO: to be completed. Depends on billing?

## [Using GitHub Actions for project management](https://docs.github.com/en/actions/guides/using-github-actions-for-project-management)
### Table of contents:
* Adding labels to issues
* Removing a label when a card is added to a project board column
* Moving assigned issues on project boards
* Commenting on an issue when a label is added
* Closing inactive issues
* Scheduling issue creation

## [Using GitHub CLI in workflows](https://docs.github.com/en/actions/guides/using-github-cli-in-workflows)
Code examples:
* [Comment action opening](https://github.com/Zolaton/zdocs/blob/f7ef081197348e04be3140b4b1e87b40ae6ef706/.github/workflows/greet-issuer.yml#L1)
* [Execute API calls through GitHub CLI](https://github.com/Zolaton/zdocs/blob/39c07c2197d672836791bf7a53e1720efcee357e/.github/workflows/remaining-open-issues.yml#L1)
