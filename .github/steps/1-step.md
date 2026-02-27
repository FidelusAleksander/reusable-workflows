## Step 1: Create your reusable workflow

Your company Octogames has been continuously shipping new web games! You, as the DevOps engineer were always tasked with creating GitHub Actions workflows for CI/CD.

You got tired of copy-pasting the same workflow logic across repositories and decided to learn how to create reusable workflows to standardize and simplify the process. In this exercise, you'll create a reusable workflow that can be called from other workflows in the same repository or even from different repositories.

<img width="900" alt="descriptive alt text" src="../images/octomatch.png" />

### 📖 Theory: What makes a workflow reusable?

A workflow becomes reusable when it is has a `workflow_call` event trigger defined in its workflow file. This lets another workflow invoke it.

Reusable workflows help you:

- Keep common CI/CD logic in one place
- Standardize behavior across repositories
- Update one file instead of many copies

In this step, you'll create a reusable workflow file that can be called in the next step.

### ⌨️ Activity: Set up your development environment

Let's use **GitHub Codespaces** to set up a cloud-based development environment and work in it for the remainder of the exercise!

1. Right-click the below button to open the **Create Codespace** page in a new tab. Use the default configuration.

   [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/{{full_repo_name}}?quickstart=1)

1. Confirm the **Repository** field is your copy of the exercise, not the original, then click the green **Create Codespace** button.

   - ✅ Your copy: `/{{full_repo_name}}`
   - ❌ Original: `/skills/reusable-workflows`

1. Wait a moment for Visual Studio Code to fully load in your browser.

### ⌨️ Activity: Create a reusable workflow

### ⌨️ Activity: Create Basic Docker Publish Workflow

Let's start off by creating a workflow to build and publish our **Stackoverflown** game as a docker image.

1. In your codespace create a new branch named exactly `reusable-workflows` and switch to it.

   ```bash
   git checkout -b reusable-workflows
   ```

1. Within the `.github/workflows` directory create a new workflow file named:

   ```text
   reusable-node-quality.yml
   ```

1. Within that file, let's start by defining the workflow name, event trigger and permissions:

    ```yaml
    name: Reusable Node Quality
    run-name: Node ${{ inputs.node-version }} Quality Checks

    on:
      workflow_call:
        inputs:
          node-version:
            description: 'Node.js version'
            required: false
            default: '20'
            type: string

    permissions:
      contents: read
    ```

   This workflow can be called from other workflows in the same repository or even from different repositories with permissions to read the repository contents and push packages to the GitHub Container Registry.

1. Add three separate jobs to the workflow: `lint`, `tests` and `e2e`.

   These are the jobs that you found yourself copy-pasting across repositories the most, so it makes sense to centralize them in a reusable workflow.

    ```yaml
    jobs:
      lint:
        name: Lint
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v6
          - uses: actions/setup-node@v6
            with:
              node-version: ${{ inputs.node-version }}
              cache: npm
          - run: npm ci
          - run: npm run lint

      tests:
        name: Tests with Coverage
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v6
          - uses: actions/setup-node@v6
            with:
              node-version: ${{ inputs.node-version }}
              cache: npm
          - run: npm ci
          - run: npm run test:coverage

      e2e:
        name: E2E Tests with Playwright
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v6
          - uses: actions/setup-node@v6
            with:
              node-version: ${{ inputs.node-version }}
              cache: npm
          - run: npm ci
          - run: npx playwright install --with-deps chromium
          - run: npm run test:e2e
    ```

   Each job checks out the repository code, sets up Node.js using the version specified in the workflow input, installs dependencies, and runs the appropriate npm script (`lint`, `test:coverage`, and `test:e2e` respectively).

1. Commit and push the `.github/workflows/reusable-node-quality.yml` file changes to the `reusable-workflows` branch.

   ```bash
   git add .github/workflows/reusable-node-quality.yml
   git commit -m "Create reusable node quality workflow"
   git push origin reusable-workflows
   ```

1. As you push your changes Mona will check your work and prepare the next step in this exercise!

<details>
<summary>Having trouble? 🤷</summary><br/>

- Make sure the file path is exactly `.github/workflows/reusable-node-quality.yml`.
- Check YAML indentation. Workflow files require spaces, not tabs.

</details>
