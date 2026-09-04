# Project Skills and Agents

This directory contains reusable GitHub Copilot agents for the OceanView Amazon Connect project. They provide focused workflows for Lambda development, service packages, code review, and AWS cost analysis.

## Available Agents

| Agent | Use it for | Example |
|---|---|---|
| [AWS Cost Review Agent](aws-cost-review.agent.md) | Estimate AWS cost drivers, identify avoidable spend, and prioritize savings across Terraform and application code. | `cost-review --scope repo --level deep` |
| [AWS Lambda Scaffolder](aws-lambda-scaffolder.agent.md) | Create a new production-ready Lambda package, including tests, Terraform wiring, and workspace registration. | `create-lambda callback_status_api "Expose callback status endpoint" API` |
| [Code Review Agent](code-review.agent.md) | Review a branch or the current Lambda package for defects, security risks, regressions, and missing tests. | `code-review 2 feature/callback-status` |
| [Create Service Scaffolder](create-service-.agent.md) | Create a shared `service_<name>` Python package for use by Lambda packages. | `create-service secrets_manager "Wrap Secrets Manager access"` |
| [Update Lambda Agent](update-lambda.agent.md) | Modify an existing Lambda package with focused implementation changes and regression-safe validation. | `update-lambda callback_status_api "Improve request validation"` |
| [Codebase Flattening Agent](codebase-flattener.agent.md) | Flatten the repository structure for easier navigation and management, ensuring all packages and modules are accessible from a single entry point. Useful for migrating the Repository's Technical Context and overall structure to OgGPT. | `flatten-codebase --scope repo` |

## Prompt Shortcuts

Prompts in [`.github/prompts`](../prompts/) provide shorter entry points for common workflows:

- [`create-lambda`](../prompts/create-lambda.prompt.md) starts the AWS Lambda Scaffolder.
- [`check`](../prompts/check.prompt.md) runs the targeted lint, type-check, test, and applicable Terraform formatting checks for a Lambda package.

## How to Use an Agent

1. Open the Copilot Chat view in VS Code.
2. Select the relevant agent, or use its command-style input in chat.
3. Provide the package name, scope, or change objective shown in the examples above.
4. Review the parsed request and confirmation summary before any workflow that changes files or analyzes costs.
5. Review the reported files, runtime impact, test results, and remaining follow-ups.

Agent files are user-invocable and are designed to preserve repository conventions. They inspect applicable instructions under `.github/instructions`, follow existing package patterns, and run focused validation for the affected area.

## Confirmation Gates

The following workflows require explicit confirmation before proceeding:

- **Create Lambda:** confirms the Lambda purpose, invocation type, name, files, and Terraform scope.
- **Update Lambda:** confirms the target package, behavior change, affected files, risks, and test impact.
- **Create Service:** confirms the service purpose, public API, package name, and planned files.
- **AWS Cost Review:** confirms repositories, environments, confidence level, and optimization goals.
- **Flatten Codebase:** confirms the repository structure, ensuring all packages and modules are accessible from a single entry point. Especially to give it to OgGPT. Take Flattened file, attach to OgGPT and Query whatever the queries are.

Code review and the `check` prompt are read-only workflows. They do not modify source, dependency, lock, or Terraform files.

## Validation Expectations

Lambda workflows normally report:

- Formatting
- Ruff linting
- `ty` type checking
- Focused tests and coverage
- Terraform formatting when infrastructure changes are involved

Service scaffolding also includes its package security check. Cost reviews distinguish code/IaC estimates from bill-backed data and must state assumptions. Code reviews report findings by severity and include security checks.

## Repository References

- [Lambda Development](../../docs/LAMBDA_DEVELOPMENT.md) — local setup and package workflow
- [Branching and Deployment](../../docs/BRANCHING_AND_DEPLOYMENT.md) — branching and CI/CD
- [Repository README](../../README.md) — platform overview and documentation index
