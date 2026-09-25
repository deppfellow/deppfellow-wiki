# notify-site.yml — secret setup and rotation

On every push to this repo's default branch (`main`), `notify-site.yml` fires a
`repository_dispatch` on the site repo, which rebuilds and deploys the site.
The site-side receiver is `.github/workflows/build.yml` in the site repo and
declares the event type this workflow sends: **`wiki-published`**. The two
strings must stay identical; this document is the assertion that they pair.

## Create the token (fine-grained PAT)

The dispatch endpoint needs `contents: write` on the target repo. Scope it to
exactly that one repository:

1. GitHub → Settings → Developer settings → Fine-grained tokens →
   **Generate new token**.
2. Repository access: **Only select repositories** → `deppfellow/deppfellow.github.io`.
3. Permissions:
   - **Contents: Read and write** (required — creating a `repository_dispatch` counts as a content write)
   - **Metadata: Read only** (mandatory on every fine-grained PAT)
4. Expiration: 90 days (custom).
5. Copy the token once; it is shown only at creation.

## Store the secret on the wiki repo

The secret must be named exactly `SITE_DISPATCH_TOKEN` on
`deppfellow/deppfellow-wiki` — the workflow reads that name.

CLI:

```sh
gh secret set SITE_DISPATCH_TOKEN -R deppfellow/deppfellow-wiki
# paste the token when prompted
```

UI: repo → Settings → Secrets and variables → Actions → **New repository
secret** → name `SITE_DISPATCH_TOKEN`, value = the token.

## Rotate (90-day cadence)

1. Create a new fine-grained PAT with the scope above (do not reuse the old
   one past expiry).
2. `gh secret set SITE_DISPATCH_TOKEN -R deppfellow/deppfellow-wiki` with the
   new value.
3. Trigger `notify-site.yml` once via **Run workflow** and confirm a site
   build run appears.
4. Delete the old token under Developer settings.
5. Record the rotation date next to this file's entry in the ticket log so the
   next expiry is predictable.

## Failure mode

The token is read from the secret at step 1 of the workflow. A missing or
expired secret fails the run loudly there (`::error::` naming
`SITE_DISPATCH_TOKEN`, exit 1) — it never fails silently. The practical effect
of an expired secret: every push to the wiki still lands in the vault, but the
site stops rebuilding until the secret is replaced. Rotation is therefore not
optional hygiene; it is what keeps the publish loop alive.
