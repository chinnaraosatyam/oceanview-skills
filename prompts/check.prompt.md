---
name: "check"
description: "Run lint, ty type checks, tests, and Terraform formatting for a Lambda package"
argument-hint: "[lambda-package-name]"
agent: "agent"
---
Run the Lambda verification workflow.

Target resolution:
- If an argument is provided, treat it as the short Lambda package name under `lambdas/packages/` (for example, `/check initiate_callback_v2`). Do not require a fully qualified path.
- If no argument is provided, identify the Lambda package containing the currently active editor file. It must be beneath `lambdas/packages/<package-name>/`.
- If neither method identifies exactly one Lambda package, ask the user to provide its short package name and do not run checks.

For the resolved package, run these commands in order:
1. `task -t /home/csatyam/projects/oceanview_amazonconnect/lambdas/packages/<package-name>/Taskfile.yaml lint`
2. `task -t /home/csatyam/projects/oceanview_amazonconnect/lambdas/packages/<package-name>/Taskfile.yaml type-check`
3. `task -t /home/csatyam/projects/oceanview_amazonconnect/lambdas/packages/<package-name>/Taskfile.yaml test`

Then, if `terraform/resources/lambdas/<package-name>/` exists, run from the repository root:
```bash
terraform fmt -check terraform/resources/lambdas/<package-name>
```

Rules:
- Run every applicable check even if an earlier check fails, unless a command cannot start.
- Do not modify source files, dependencies, lock files, generated coverage files, or Terraform files.
- Do not run deployment, build, security, or unrelated package checks.
- Use minimal context and output: do not read files or explain commands unless needed to resolve the target or diagnose a failure.
- Finish with only a compact Markdown table containing the package name, each check, and its pass, fail, or skipped status. Add one short failure detail only when a check fails.
