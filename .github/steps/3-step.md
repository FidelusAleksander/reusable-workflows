## Step 3: Add inputs and outputs

In this final step, you will make your reusable workflow configurable and return useful data back to the caller.

### 📖 Theory: Inputs and outputs in reusable workflows

Inputs define values the caller can pass into the reusable workflow using `with`. Outputs define values the reusable workflow returns to the caller after execution.

A common pattern is to expose a summary value from tests, then use it in a follow-up job in the caller workflow.

### ⌨️ Activity: Pass `node-version` and publish `coverage-percent`

1. Open `.github/workflows/reusable-node-quality.yml`.
1. In `on.workflow_call`, ensure there is an input named `node-version` and add a workflow output named `coverage-percent`.
1. In the tests job, capture coverage percent from the coverage summary file and map it to a job output.
1. Open `.github/workflows/ci.yml` and, in the caller job, pass `node-version` with `with:`.
1. Add a follow-up job that reads `{% raw %}${{ needs.quality.outputs.coverage-percent }}{% endraw %}` and posts it in a pull request comment.
1. Merge the pull request into `main` to finish the exercise.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Workflow outputs in `workflow_call` must map to job-level outputs.
- Caller jobs can read reusable workflow outputs with `needs.<job-id>.outputs.<name>`.
- If you post a pull request comment, ensure the job has `pull-requests: write` permission.

</details>
