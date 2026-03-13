# github-action-terraform-plan-apply

A composite GitHub Action for running Terraform plan and apply with workspace support, multi-target plans, loop mode over tfvars files, and intelligent error handling.

## Usage

### Single Workspace

```yaml
- uses: asaphe/github-action-terraform-plan-apply@v1
  with:
    working-directory: devops/terraform/my-module
    workspace: production
    var-file: vars/production.tfvars
```

### Plan Only (Dry Run)

```yaml
- uses: asaphe/github-action-terraform-plan-apply@v1
  with:
    working-directory: devops/terraform/my-module
    workspace: staging
    var-file: vars/staging.tfvars
    plan-only: "true"
```

### Loop Over tfvars Directory

```yaml
- uses: asaphe/github-action-terraform-plan-apply@v1
  with:
    working-directory: devops/terraform/my-module
    workspace: prod
    var-file: vars/
    loop-over-tfvars: "true"
```

### With Targeted Resources

```yaml
- uses: asaphe/github-action-terraform-plan-apply@v1
  with:
    working-directory: devops/terraform/my-module
    workspace: production
    var-file: vars/production.tfvars
    plan-targets: "module.rds module.security_group"
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Directory to run Terraform in | Yes | - |
| `workspace` | Terraform workspace name (or base name for loop) | Yes | - |
| `var-file` | Path to tfvars file(s) or directory | No | - |
| `plan-targets` | Space-separated list of Terraform targets | No | - |
| `parallelism` | Parallelism for apply | No | - |
| `plan-only` | If true, only run plan | No | `false` |
| `loop-over-tfvars` | If true, loop over all tfvars files | No | `false` |

## Features

- Workspace auto-creation (`-or-create=true`)
- Multiple var-file support (newline-separated)
- Loop mode: iterates over all `.tfvars` files in a directory, creating `{workspace}-{filename}` workspaces
- Targeted plans with `-target` flags
- Automatic retry on Karpenter hash annotation inconsistency (known provider issue)
- Actionable error messages for common AWS errors (EntityAlreadyExists, AccessDenied, ResourceNotFound, ValidationError)

## License

Apache 2.0 - See [LICENSE](LICENSE) for details.
