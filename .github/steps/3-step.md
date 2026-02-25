## Step 3: Add inputs to make the workflow flexible

Reusable workflows are most useful when they support configurable behavior through inputs.

### 📖 Theory: Inputs in `workflow_call`

Inputs let caller workflows pass values into the reusable workflow. Define inputs in the `workflow_call` block and reference them through the `inputs` context.

This makes one reusable workflow work for many use cases without copying files.

### ⌨️ Activity: Add and use a `node-version` input

1. Open `.github/workflows/reusable-ci.yml`.

1. In `on.workflow_call`, add an input named `node-version`:

   ```yaml
   on:
     workflow_call:
       inputs:
         node-version:
           required: false
           type: string
           default: "20"
   ```

1. In the reusable job, add a step that prints the selected version:

   ```yaml
   - name: Show selected Node version
     run: echo "Node version: ${{ inputs.node-version }}"
   ```

1. Open `.github/workflows/ci.yml` and pass `node-version: '20'` with `with:` under the `reusable` job.

   <img width="200" alt="Mona with jetpack" src="../images/jetpacktocat.png" />

1. Commit the changes to `main` to finish the exercise.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Keep input definitions under `on.workflow_call.inputs`.
- Use quotes around version strings like `'20'` to avoid YAML parsing surprises.

</details>
