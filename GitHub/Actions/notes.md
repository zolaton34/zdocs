# Action Notes

## TODO
* [Skipping workflow runs](https://docs.github.com/en/actions/guides/about-continuous-integration#skipping-workflow-runs). If we add a [skip ci] for the tip commit of pr, workflow are skipped before the pr is merge. They are however run after the run. 
  * _We may want to prevent any skip ci on the pull request_ while still make it possible on simple pushes.
    * We can require specific checks before merging PRs this will prevent skipping these specific workflows to run before the merge. TODO check if it is cumbersome.
* Search of status check on the UI > Protected branches is based on the job name, not the worklow name. TODO check what happens when several jobs have the same name.