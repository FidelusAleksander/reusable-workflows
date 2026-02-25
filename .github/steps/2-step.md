## Step 2: Call the reusable workflow

Now that your reusable workflow exists, you can call it from another workflow instead of duplicating steps.

### 📖 Theory: Calling reusable workflows

<!-- GitHub-styled notifications can be used outside of ordered lists. Available options are: NOTE, IMPORTANT, WARNING, TIP, CAUTION -->
<!--
> [!NOTE]
> (Important note or additional information relevant to this section)
 -->

A caller workflow references a reusable workflow with `uses` at the job level. For a reusable workflow in the same repository, use a relative path like `./.github/workflows/reusable-ci.yml`.

This pattern separates orchestration from implementation:

- Caller workflow: chooses when CI runs
- Reusable workflow: defines what CI does

<img width="200" alt="descriptive alt text" src="../images/inflatocat.png" />

### ⌨️ Activity: Add `.github/workflows/ci.yml` to call your reusable workflow

1. Create a new file at `.github/workflows/ci.yml`.

1. Add this workflow:

   ```yaml
   name: CI

   on:
     push:
       branches:
         - main
     pull_request:

   jobs:
     reusable:
       uses: ./.github/workflows/reusable-ci.yml
   ```

1. Commit your changes to `main`.

1. Wait for this step to be checked automatically.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Ensure `uses` is under `jobs.<job_id>`, not inside `steps`.
- Confirm the path is exactly `./.github/workflows/reusable-ci.yml`.

</details>
