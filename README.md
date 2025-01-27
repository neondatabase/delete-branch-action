# 🗑️ Neon Delete Branch Action

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./docs/logos/neon-logo-dark.svg">
    <img alt="Neon logo" src="./docs/logos/neon-logo-light.svg">
  </picture>
</p>

This action deletes a specified Neon branch within your Neon project.

It's designed for workflows where you need to programmatically remove Neon branches, such as in cleanup processes after testing.

## Setup

Using the action requires adding a Neon API key to your GitHub Secrets. There are two ways you can perform this setup:

- **Using the Neon GitHub Integration** (recommended 👍) — this integration connects your Neon project to your GitHub repository, creates an API key, and sets the API key in your GitHub repository for you. See [Neon GitHub Integration](https://neon.tech/docs/guides/neon-github-integration) for instructions.
- **Manual setup** — this method requires obtaining a Neon API key and configuring it manually in your GitHub repository.

  1. **Obtain a Neon API key.** See [Create an API key](https://neon.tech/docs/manage/api-keys#create-an-api-key) for instructions on the Neon documentation.
  2. In your GitHub repository, go to **Settings** and locate **Secrets and variables** at the bottom of the left sidebar.
  3. Click **Actions** > **New repository secret**.
  4. Name the secret `NEON_API_KEY` and paste your API key in the **Value** field.
  5. Click **Add secret**.

## Usage

The following fields are required to run the Delete Branch action:

- `project_id` — The Neon project ID. If you have the Neon GitHub Integration installed, you can specify `${{ vars.NEON_PROJECT_ID }}`. You can find the project ID of your Neon project on the Settings page of your Neon console.
- `api_key` — The Neon API key for your Neon project or organization. If you have the GitHub integration installed, specify `${{ secrets.NEON_API_KEY }}`.
- `branch` or `branch_id` — Specifies the branch to delete. You can use either the `branch` name or the `branch_id`. Using `branch` (name or ID) is recommended, `branch_id` is deprecated but still supported for backward compatibility.

Setup the action in your workflow:

```yml
steps:
  - uses: neondatabase/delete-branch-action@v3
    with:
      project_id: your_neon_project_id
      branch: my-feature-branch # Specify branch name or ID here
      api_key: ${{ secrets.NEON_API_KEY }}
```

Alternatively, you can use `${{ vars.NEON_PROJECT_ID }}` to get your `project_id`. If you have set up the [Neon GitHub Integration](https://neon.tech/docs/guides/neon-github-integration), the `NEON_PROJECT_ID` variable will be defined as a variable in your GitHub repository.

You can specify the branch to delete using either `branch` (name or ID - recommended) or `branch_id` (ID only - deprecated). The `branch` input is more flexible as it accepts both names and IDs.

```yml
# Example using branch name
steps:
  - uses: neondatabase/delete-branch-action@v3
    with:
      project_id: ${{ vars.NEON_PROJECT_ID }}
      branch: staging-branch
      api_key: ${{ secrets.NEON_API_KEY }}

# Example using branch ID (less readable, but still supported)
steps:
  - uses: neondatabase/delete-branch-action@v3
    with:
      project_id: ${{ vars.NEON_PROJECT_ID }}
      branch: br-some-branch-id-1234 # You can also use branch ID in `branch` input
      api_key: ${{ secrets.NEON_API_KEY }}

# Example using deprecated branch_id (only branch ID, less flexible)
steps:
  - uses: neondatabase/delete-branch-action@v3
    with:
      project_id: ${{ vars.NEON_PROJECT_ID }}
      branch_id: br-some-branch-id-1234 # Deprecated input, use `branch` instead
      api_key: ${{ secrets.NEON_API_KEY }}
```

## Outputs

This action does not provide any outputs. The primary function is to delete a Neon branch.

## Example Workflow

Here's an example of a complete GitHub Actions workflow that deletes a Neon branch:

```yml
name: Neon Github Actions Delete Branch

on:
  # You can modify the following line to trigger the workflow on a different event, such as `push` or `pull_request`, as per your requirements. We have used `workflow_dispatch` for triggering the action in this example.
  workflow_dispatch:

jobs:
  Delete-Neon-Branch:
    runs-on: ubuntu-24.04
    steps:
      - uses: neondatabase/delete-branch-action@v3
        with:
          project_id: ${{ vars.NEON_PROJECT_ID }}
          branch: actions_reusable # Replace with the branch name or ID you want to delete
          api_key: ${{ secrets.NEON_API_KEY }}
      - run: echo "actions_reusable branch deleted successfully"
```

---

## Other Actions

Check out other Neon GitHub Actions:

- [Create Branch Action](https://github.com/neondatabase/create-branch-action)
- [Reset Branch Action](https://github.com/neondatabase/reset-branch-action)
- [Schema Diff Action](https://github.com/neondatabase/schema-diff-action)
