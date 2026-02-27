## Step 2: Let's use the reusable workflow!

Nice! You've finally created a reusable workflow and you're ready to use it.

In this step, you'll create a caller workflow that runs on pull requests and delegates quality checks to that reusable workflow.

### 📖 Theory: How to use reusable workflows?

In this exercise, you will call a reusable workflow from the same repository, but you can also call reusable workflows from other repositories to share standards across teams.

| Reusable workflow location | `uses` syntax                                   | Typical use case                                                         |
| -------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------ |
| Same repository            | `./.github/workflows/reusable-node-quality.yml` | Keep workflow logic close to one project                                 |
| Different repository       | `owner/repo/.github/workflows/workflow.yml@ref` | Share standardized workflows across many repositories in an organization |

When using a reusable workflow from another repository, pin `@ref` to a stable tag or SHA for predictable behavior.

### ⌨️ Activity: Create a CI workflow and call your reusable workflow

1. Open `.github/workflows/ci.yml`.
1. Replace the file contents with this workflow:

   ```yaml
   name: CI

   on:
     pull_request:
       branches:
         - main

   jobs:
     quality:
       uses: ./.github/workflows/reusable-node-quality.yml
        with:
          node-version: 20
   ```

### ⌨️ Activity: Commit, push, and open a pull request

1. Commit your `ci.yml` changes to the `reusable-workflows` branch.
1. Push the branch to GitHub.
1. Open or update a pull request from `reusable-workflows` into `main`.
1. Open the pull request **Checks** tab and confirm the CI workflow starts running.
1. Wait for this step to be checked automatically.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Keep `uses` directly under `jobs.<job_id>`.
- Confirm the filename is `reusable-node-quality.yml`.
- Make sure your pull request targets `main`.

</details>
