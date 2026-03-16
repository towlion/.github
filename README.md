# towlion/.github

Organization-level configuration, community health files, and reusable workflows.

## Reusable Workflows

All reusable workflows use `workflow_call` and live in `.github/workflows/`.

### validate.yml

Runs the Towlion spec validator against a repository.

**Inputs:**
| Input | Type | Default | Description |
|---|---|---|---|
| `tier` | number | `2` | Spec validation tier (1-3) |

**Example caller:**
```yaml
jobs:
  validate:
    uses: towlion/.github/.github/workflows/validate.yml@main
```

### test-python.yml

Runs Python tests with pip caching. Installs `requirements.txt` + `pytest`, runs tests if any exist.

**Inputs:**
| Input | Type | Default | Description |
|---|---|---|---|
| `python-version` | string | `"3.12"` | Python version to use |

**Example caller:**
```yaml
jobs:
  test:
    uses: towlion/.github/.github/workflows/test-python.yml@main
```

### deploy.yml

Deploys an application to the production server via SSH. Handles database creation, credential sourcing, Docker Compose build, Trivy scan, Alembic migration, health check, and Caddyfile generation.

**Inputs:**
| Input | Type | Required | Description |
|---|---|---|---|
| `caddyfile-template` | string | yes | Caddyfile template (see placeholders below) |
| `use-deployment-api` | boolean | no (default: true) | Create GitHub deployment status |

**Secrets:** `SERVER_HOST`, `SERVER_USER`, `SERVER_SSH_KEY`, `APP_DOMAIN`

**Caddyfile template placeholders:**
- `__APP_DOMAIN__` — replaced with the `APP_DOMAIN` secret value
- `__APP_NAME__` — replaced with the repository name

**Example caller (simple app):**
```yaml
jobs:
  test:
    uses: towlion/.github/.github/workflows/test-python.yml@main

  deploy:
    needs: test
    uses: towlion/.github/.github/workflows/deploy.yml@main
    with:
      caddyfile-template: |
        __APP_DOMAIN__ {
            import security_headers
            reverse_proxy __APP_NAME__-app-1:8000
        }
    secrets:
      SERVER_HOST: ${{ secrets.SERVER_HOST }}
      SERVER_USER: ${{ secrets.SERVER_USER }}
      SERVER_SSH_KEY: ${{ secrets.SERVER_SSH_KEY }}
      APP_DOMAIN: ${{ secrets.APP_DOMAIN }}
```

**Example caller (multi-service app like WIT):**
```yaml
  deploy:
    needs: test
    uses: towlion/.github/.github/workflows/deploy.yml@main
    with:
      caddyfile-template: |
        __APP_DOMAIN__ {
            import security_headers
            handle /api/* {
                reverse_proxy __APP_NAME__-app-1:8000
            }
            handle /health {
                reverse_proxy __APP_NAME__-app-1:8000
            }
            handle {
                reverse_proxy __APP_NAME__-frontend-1:3000
            }
        }
    secrets:
      SERVER_HOST: ${{ secrets.SERVER_HOST }}
      SERVER_USER: ${{ secrets.SERVER_USER }}
      SERVER_SSH_KEY: ${{ secrets.SERVER_SSH_KEY }}
      APP_DOMAIN: ${{ secrets.APP_DOMAIN }}
```

### preview.yml

Deploys preview environments for pull requests. Handles per-PR schema isolation, preview directory creation, and cleanup on PR close.

**Inputs:**
| Input | Type | Required | Description |
|---|---|---|---|
| `caddyfile-template` | string | yes | Caddyfile template (see placeholders below) |

**Secrets:** `SERVER_HOST`, `SERVER_USER`, `SERVER_SSH_KEY`, `PREVIEW_DOMAIN`

**Preview-specific placeholders** (in addition to `__APP_DOMAIN__` and `__APP_NAME__`):
- `__PR_NUMBER__` — the PR number
- `__PREVIEW_DOMAIN__` — the `PREVIEW_DOMAIN` secret value

Note: `__APP_DOMAIN__` is set to `pr-<N>.preview.<PREVIEW_DOMAIN>` and `__APP_NAME__` is set to `<repo>-pr-<N>` automatically.

**Example caller:**
```yaml
jobs:
  preview:
    uses: towlion/.github/.github/workflows/preview.yml@main
    with:
      caddyfile-template: |
        __APP_DOMAIN__ {
            import security_headers
            reverse_proxy __APP_NAME__-app-1:8000
        }
    secrets:
      SERVER_HOST: ${{ secrets.SERVER_HOST }}
      SERVER_USER: ${{ secrets.SERVER_USER }}
      SERVER_SSH_KEY: ${{ secrets.SERVER_SSH_KEY }}
      PREVIEW_DOMAIN: ${{ secrets.PREVIEW_DOMAIN }}
```

## Organization Profile

The `profile/README.md` is displayed on the [Towlion GitHub org page](https://github.com/towlion).
