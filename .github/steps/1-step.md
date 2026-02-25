## Step 1: Create your reusable workflow

Your team has multiple repositories running similar CI checks. To avoid copying the same YAML in every repo, you'll centralize the shared logic in one reusable workflow.

<img width="200" alt="descriptive alt text" src="../images/inspectocat.png" />

### 📖 Theory: What makes a workflow reusable?

<!-- GitHub-styled notifications can be used outside of ordered lists. Available options are: NOTE, IMPORTANT, WARNING, TIP, CAUTION -->
<!--
> [!NOTE]
> (Important note or additional information relevant to this section)
 -->

A workflow becomes reusable when it is triggered by `workflow_call`. This lets another workflow invoke it as a job.

Reusable workflows help you:

- Keep common CI/CD logic in one place
- Standardize behavior across repositories
- Update one file instead of many copies

In this step, you'll create a reusable workflow file that can be called in the next step.

### ⌨️ Activity: Add `.github/workflows/reusable-ci.yml`

1. Create a new file at `.github/workflows/reusable-ci.yml`.

1. Add this workflow skeleton:

   ```yaml
   name: Reusable CI

   on:
     workflow_call:

   jobs:
     lint:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v6
         - run: echo "Reusable CI is running"
   ```

1. Commit directly to the `main` branch.

1. Wait a few seconds for Mona to post the next step.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Make sure the file path is exactly `.github/workflows/reusable-ci.yml`.
- Check YAML indentation. Workflow files require spaces, not tabs.

</details>
