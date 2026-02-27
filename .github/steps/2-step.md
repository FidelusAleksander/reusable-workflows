## Step 2: Create a caller workflow

You now have a reusable workflow. Next, create a caller workflow that runs on pull requests and delegates quality checks to that reusable workflow.

### 📖 Theory: Caller workflows and permissions

In this exercise, you will call a reusable workflow from the same repository, but you can also call reusable workflows from other repositories to share standards across teams.

| Reusable workflow location | `uses` syntax                                   | Typical use case                                                         |
| -------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------ |
| Same repository            | `./.github/workflows/reusable-node-quality.yml` | Keep workflow logic close to one project                                 |
| Different repository       | `owner/repo/.github/workflows/workflow.yml@ref` | Share standardized workflows across many repositories in an organization |

When using a reusable workflow from another repository, pin `@ref` to a stable tag or SHA for predictable behavior.

Think of permissions like an access pass:

- The **caller workflow** decides what access is allowed.
- The **reusable workflow** can only use that allowed access.
- A called workflow cannot ask for more access than the caller gives it.

Always set permissions to the least access required by your workflow.

### ⌨️ Activity: Update `ci.yml` to call your reusable workflow

1. Open `.github/workflows/ci.yml`.
1. Ensure a workflow job calls your reusable workflow using:

   ```yaml
   uses: ./.github/workflows/reusable-node-quality.yml
   ```

1. In that same caller job, set explicit permissions:

   ```yaml
   permissions:
     contents: read
   ```

1. Commit your changes to the `reusable-workflows` branch.
1. Open or update a pull request from `reusable-workflows` into `main`.
1. Wait for this step to be checked automatically.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Keep `uses` directly under `jobs.<job_id>`.
- Confirm the filename is `reusable-node-quality.yml`.
- Make sure your pull request targets `main`.

</details>
