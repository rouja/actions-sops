# Actions-sops

**action-sops** is a github action that install sops (thanks to [nhedger](https://github.com/nhedger/setup-sops)) and loads secrets from a sops encrypted file with age key into $GITHUB_ENV variable.

## Inputs

```yaml
- name: Load sops secrets
  uses: rouja/actions-sops@main
  with:
    # Path where the sops encrypted file is in the repository
    secret-file: .github/workflows/secrets.enc.env
    # Private age key to decrypt the sops file
    age-key: ${{ secrets.SOPS_PRIVATE }}
```

The sops encrypted file should be like this : 

```bash
# .github/workflows/secrets.enc.env
SUPER_SECRET="changeme1"
ANOTHER_SECRET="changeme2"
```

## Exemple

```yaml
name: Demo Workflow

on:
  push:

jobs:
  sops-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Load sops secrets
        uses: rouja/actions-sops@main
        with:
          secret-file: .github/workflows/secrets.enc.env
          age-key: ${{ secrets.SOPS_PRIVATE }}

      - name: Print value
        run: |
          echo $SUPER_SECRET
          echo $ANOTHER_SECRET
```
