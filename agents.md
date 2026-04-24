# AI Agent Guidance for CDK Terrain

This file provides instructions and context for AI agents working on the CDK Terrain (CDKTN) repository.

## Project Overview

CDK Terrain is a community fork of the Cloud Development Kit for Terraform (CDKTF). It allows users to define cloud infrastructure using familiar programming languages and provision it via Terraform or OpenTofu.

### Core Technology Stack

- **Language:** Primarily TypeScript.
- **Multi-language Support:** [JSII](https://github.com/aws/jsii) is used to transpile TypeScript into Python, Go, Java, and C#.
- **Monorepo Management:** Lerna and Yarn Workspaces.
- **CLI:** Node.js based CLI using `yargs` and `ink` for UI components.

## Repository Structure

- `packages/cdktn`: The core library containing constructs (`TerraformStack`, `TerraformResource`, etc.). **Uses JSII.**
- `packages/cdktn-cli`: CLI entry point and wrapper.
- `packages/@cdktn/cli-core`: Main implementation logic for the CLI.
- `packages/@cdktn/provider-generator`: Generates language bindings from Terraform provider schemas.
- `packages/@cdktn/hcl2cdk`: Logic for converting HCL to CDK code.
- `packages/@cdktn/commons`: Shared utilities.
- `test/`: Integration tests.
- `examples/`: Example projects in various supported languages.

## Key Workflows for Agents

### 1. Building the Project

Always build from the root unless working on a specific isolated package.

```bash
yarn build            # Build all packages (tsc + jsii)
yarn watch            # Watch mode for development
yarn package          # Build + create distributable packages for all languages
```

### 2. Testing

- **Unit Tests:** `yarn test` (runs jest in all packages).
- **Integration Tests:** Require `yarn package` first.
  - `yarn integration` (all)
  - `yarn integration:typescript` (specific language)

### 3. Adding New Features to the Library

If you add a public API to `packages/cdktn`, it MUST be JSII-compliant.
- Avoid using `any`, `unknown` (in public APIs), or complex TypeScript types that JSII cannot represent.
- Ensure proper JSDoc comments as they are used for documentation generation in all languages.

### 4. CLI Development

- CLI implementation is in `@cdktn/cli-core`.
- `cdktn-cli` is the distribution wrapper.
- When testing CLI changes locally: `./packages/cdktn-cli/bundle/bin/cdktn <command>`

## Best in Class Production Standards

- **Security:** Ensure no vulnerabilities in dependencies. Use `yarn resolutions` in the root `package.json` for transitive dependency fixes.
- **Observability:**
  - The CLI uses Sentry for error tracking (see `packages/@cdktn/cli-core/src/lib/error-reporting.ts`).
  - Log levels are managed via `CDKTF_LOG_LEVEL`.
- **Testing:** Aim for high unit test coverage in `packages/cdktn` and `@cdktn/cli-core`. Always add integration tests for new CLI commands.
- **Documentation:** Keep `README.md` and `CLAUDE.md` up to date. JSDoc in the core library is mandatory.

## Project Evolution (CDKTF -> CDKTN)

This project is a fork. Many internal symbols still refer to `cdktf` for compatibility reasons. Do not blindly rename `cdktf` to `cdktn` in internal logic (especially logical IDs or serialization formats) unless instructed, as it may break existing state files for users.

## Agentic Tools

Refer to `.agents/` directory for specific skill sets and commands available to sophisticated agents.
Refer to `CLAUDE.md` for quick reference on builds and common tasks.
