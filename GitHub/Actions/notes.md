# Action Notes

## TODO
* [Skipping workflow runs](https://docs.github.com/en/actions/guides/about-continuous-integration#skipping-workflow-runs). If we add a [skip ci] for the tip commit of pr, workflow are skipped before the pr is merge. They are however run after the run. 
  * _We may want to prevent any skip ci on the pull request_ while still make it possible on simple pushes.
    * We can require specific checks before merging PRs this will prevent skipping these specific workflows to run before the merge.
    * Actually requiring one specific check is enough to block the merge. 
    * The requester needs just to commit with a new message without the skip ci comment on his branch. Then the pull request manager can approve the workflows and if reviewers are required, approve again the pull request.
    * Take note that with __one specific check__ everything is blocked on the pr, but when enabling with the new comment the approval is for __all the workflows__
    * SUGGESTION: Always add at least one check to block merges with skip CI. (TODO: what if we do require manual approval of workflows ?)
* Search of status check on the UI > Protected branches is based on the job name, not the worklow name. 