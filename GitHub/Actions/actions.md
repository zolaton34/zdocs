# [Creating Actions](https://docs.github.com/en/actions/creating-actions)
## [Docker container actions](https://docs.github.com/en/actions/creating-actions/about-actions#docker-container-actions)
* For better consistency and reliability
* Slower than other actions

## [JavaScript actions](https://docs.github.com/en/actions/creating-actions/about-actions#javascript-actions)
* Run directly on the runner
* Must be pure Javascript (no other binaries)

## [Composite run steps actions](https://docs.github.com/en/actions/creating-actions/about-actions#composite-run-steps-actions)
* Bundle different step into a single action. For example bundle run commands as a single step.

## [Choosing a location for your action](https://docs.github.com/en/actions/creating-actions/about-actions#choosing-a-location-for-your-action)
* In its own repository if shared
* If not shared recommandation is to put them under `.github/actions`