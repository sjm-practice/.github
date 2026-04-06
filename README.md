# .github
GitHub Templates for Organization Projects
## Resources
- https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates
- https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file
## Workflows
### PR Approval On Comment
A workflow was created to automatically approve a PR, when a top level '-APPROVE-' comment was added to the PR by the owner. Note, GitHub Action approvals do not count toward required approvals for merging. So repo branch rules for this organization (or individual repos) will be set to zero. The '-APPROVAL-' comment action can still be used for audit trail purposes.

#### Required Repository Setting

To make this work end-to-end, each repository will need **one critical repository setting**:

**Allow GitHub Actions to create and approve pull requests** - This is controlled by a **Branch Protection Rule** on the branches where you want auto-approval to work.

Specifically, you need to ensure that:

1. **Go to**: Repository Settings → Branches → Branch Protection Rules
2. **For the branch(es)** where you want auto-approval (typically `main`), ensure:
   - ✅ **Require pull request reviews before merging** is enabled (if you have this rule)
   - ✅ **Dismiss stale pull request approvals when new commits are pushed** - optional, but recommended
   - ⚠️ **Do NOT require approval from code owners** if you're the only approver, or make sure your bot/workflow user is listed as a code owner

3. **Critical**: Make sure the branch rule **doesn't dismiss automated approvals** or has an exception for GitHub Actions

#### Additional Check: Workflow Permissions

In your **repository settings**, go to:
- **Settings → Actions → General → Workflow permissions**
- Ensure it's set to either:
  - ✅ **"Read and write permissions"** (recommended for auto-approval)
  - ✅ **"Read repository contents permission"** with **"Allow GitHub Actions to create and approve pull requests"** enabled

#### Potential Issue with Your Setup

One thing to note: In your reusable workflow, the approval is done with `secrets.GITHUB_TOKEN`. By default, GITHUB_TOKEN **cannot approve PRs in branch protection rules** because GitHub treats self-approval differently for security reasons.

**To work around this**, you may need to:
1. Use a **Personal Access Token (PAT)** instead of `GITHUB_TOKEN`, OR
2. In branch protection rules, disable **"Require approval from pull request reviewers"** if you only want automated approval without manual review bypass
