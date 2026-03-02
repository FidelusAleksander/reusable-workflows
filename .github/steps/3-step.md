## Step 3: Add permissions-aware deployment and PR feedback

Great progress! You already call your reusable quality workflow from `ci.yml`.

In this final step, you'll expand the caller workflow with two more jobs:

- `github-pages` to deploy your app
- `comment` to post deployment details on the pull request

### 📖 Theory: How permissions pass to reusable workflows

When a workflow calls a reusable workflow, permissions from the caller context are what matter.

- The called workflow can **use only the permissions it receives** from the caller run.
- You can define permissions at the workflow level and then override per job when needed.
- Keep permissions minimal (principle of least privilege): give each job only what it needs.

In this step, the deployment job needs Pages permissions, while the PR comment job needs permission to write pull request comments.

### ⌨️ Activity: Extend `.github/workflows/ci.yml` with deployment and PR comment jobs

1. Open `.github/workflows/ci.yml`.
1. Update it so it includes all three jobs: `quality`, `github-pages`, and `comment`.
1. Replace the file content with:

```yaml
name: CI

on:
  pull_request:
    branches:
      - main

permissions:
  contents: read

jobs:
  quality:
    name: Quality checks
    uses: ./.github/workflows/reusable-node-quality.yml
    with:
      node-version: "24"

  github-pages:
    name: Deploy to GitHub Pages
    needs: quality
    permissions:
      contents: read
      pages: write
      id-token: write
    uses: ./.github/workflows/deploy-pages.yml
    with:
      node-version: "24"

  comment:
    name: Comment on PR
    if: always()
    needs: [quality, github-pages]
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      - name: Comment with Pages URL
        uses: GrantBirki/comment@v2.1.1
        with:
          issue-number: {% raw %}${{ github.event.pull_request.number }}{% endraw %}
          body: |
            ✅ **Workflow completed**

            **Workflow run:** https://github.com/{% raw %}${{ github.repository }}{% endraw %}/actions/runs/{% raw %}${{ github.run_id }}{% endraw %}

            **GitHub Pages URL:** {% raw %}${{ needs.github-pages.outputs.page_url }}{% endraw %}
```

### ⌨️ Activity: Commit, push, and merge your pull request

1. Commit your `ci.yml` changes to the `reusable-workflows` branch.
1. Push the branch.
1. Open or update your pull request from `reusable-workflows` into `main`.
1. Wait for the `CI` workflow to complete.
1. Refresh the issue and wait for this step to be checked automatically.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Make sure the job id is `github-pages` (with a hyphen), not `pages`.
- In `comment`, include `if: always()` so the comment job always runs.
- Confirm the `CI` workflow name is exactly `CI` in `.github/workflows/ci.yml`.

</details>
