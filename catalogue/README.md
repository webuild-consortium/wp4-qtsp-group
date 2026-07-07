# Catalogue Site

This directory is the source for the [GitHub Pages website](https://webuild-consortium.github.io/wp4-qtsp-group/).

## Contributing content

The catalogue entries are maintained in [`catalogue/catalogue.md`](./catalogue.md), not in `index.html`.

Each organisation is one section in that file. Entries are ordered alphabetically by organisation name. To add or update your organisation, copy the template below and fill it in, keeping the heading levels exactly as shown:

```markdown
## Your Organisation Name

### About

A short description of your organisation.

### Contact Point(s)

- [Name](mailto:you@example.com)

### Trust Services

#### Qualified Electronic Attestation of Attributes (QeAA)

##### Documentation

[Link text](https://example.com/docs)

##### Supported Attestations

###### Hello World Attestation

**Status**

Live

**Description**

A short description of the attestation.

**Supported formats**

- SD-JWT
- mdoc
```

Use `TODO` as a placeholder for anything not yet available.

## How to submit a change (fork & pull request)

You do not have write access to the main repository, so changes are submitted from your own **fork**. This is the step where people most often get stuck, so here it is in full.

The main ("upstream") repository is [`webuild-consortium/wp4-qtsp-group`](https://github.com/webuild-consortium/wp4-qtsp-group).

### Option A — entirely in the GitHub web UI (no tools needed)

1. Open [`catalogue/catalogue.md`](https://github.com/webuild-consortium/wp4-qtsp-group/blob/main/catalogue/catalogue.md) on GitHub.
2. Click the **pencil (✏️) "Edit this file"** icon. GitHub will tell you that you need to fork the repository to propose changes — click **"Fork this repository"**. This creates a copy under your own account (e.g. `your-username/wp4-qtsp-group`).
3. Make your edits in the editor, inserting your organisation in alphabetical order.
4. At the bottom, under **"Commit changes"**, write a short message (e.g. `Add <Organisation> to catalogue`) and choose **"Create a new branch and start a pull request"**.
5. Click **Propose changes**, then **Create pull request**. Confirm the PR targets `webuild-consortium/wp4-qtsp-group` `main` and submit.

### Option B — using Git and the GitHub CLI locally

```bash
# 1. Fork the upstream repo to your account (creates your-username/wp4-qtsp-group)
gh repo fork webuild-consortium/wp4-qtsp-group --clone
cd wp4-qtsp-group

# 2. Create a branch for your change
git checkout -b docs/add-my-organisation

# 3. Edit catalogue/catalogue.md, then commit
git add catalogue/catalogue.md
git commit -m "Add <Organisation> to catalogue"

# 4. Push to your fork and open the pull request against upstream
git push -u origin docs/add-my-organisation
gh pr create --repo webuild-consortium/wp4-qtsp-group --base main --fill
```

Without the GitHub CLI, the equivalent manual steps are: click **Fork** on the upstream repo page, `git clone` your fork, create a branch, commit, `git push`, then open the fork on GitHub and click **"Compare & pull request"**.

A maintainer will review and merge your pull request. Once merged to `main`, the site redeploys automatically (see Deployment below).

## Local preview

Use a static server (recommended) so markdown loading works:

```bash
cd catalogue
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deployment

A GitHub Actions workflow in `.github/workflows/pages.yml` deploys this directory to GitHub Pages on pushes to `main`.
