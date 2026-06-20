# AGENTS.md

## Project Purpose

This repository contains a public catalogue of services offered by QTSPs
participating in the WE BUILD project.

The catalogue is intended to:

- present participating QTSPs and their offerings clearly;
- help use case groups discover relevant trust services and capabilities;
- remain simple to publish as a static single-page application on GitHub Pages;
- stay easy to maintain and contribute to, including for contributors with
  limited technical experience.

## Product Scope

The catalogue should allow each listed QTSP to present multiple trust services
or functional roles. Examples include:

- QEAA issuer
- verifier
- QES provider

The structure should remain flexible so more service types and capabilities can
be added in the future without major rework.

## Implementation Expectations

- The catalogue should remain a simple static site.
- It should work well as a single-page application hosted on GitHub Pages.
- It should support a dedicated single-QTSP view on top of the current
  catalogue functionality.
- Each QTSP should have a stable identifier that can be used in the URL, for
  example through a query parameter such as `?id=<qtsp-id>`.
- When a valid QTSP identifier is present in the URL, the application should
  show only that QTSP's page or detail view.
- When no QTSP identifier is present, the application should continue to show
  the normal catalogue overview.
- The contribution flow should favor low-complexity edits over heavy tooling.
- Prefer approaches that allow non-technical contributors to update content with
  minimal risk.

## Contribution Principles

When updating this project:

- prioritize readability over cleverness;
- keep the content structure easy to understand;
- avoid introducing unnecessary build steps or deployment complexity;
- preserve compatibility with GitHub Pages hosting;
- make it straightforward to add, edit, or remove QTSP entries and their
  service offerings.
- keep interoperability test results as a single source of truth outside this
  catalogue when they are maintained centrally.

## How to update the catalogue

Catalogue content lives in [`catalogue/catalogue.md`](catalogue/catalogue.md), where each
organisation is one `##` section, ordered alphabetically by name. Edits to
`index.html` are not needed — the page renders the markdown at load time.

Contributors typically do not have write access, so changes are submitted via a
**fork and pull request** against the upstream repository
`webuild-consortium/wp4-qtsp-group`:

1. Fork the upstream repo to your own account (the GitHub "Edit this file" pencil
   offers to do this automatically, or run `gh repo fork webuild-consortium/wp4-qtsp-group --clone`).
2. Add or edit your organisation's section in `catalogue/catalogue.md`, following
   the entry template and keeping entries alphabetical. Use `TODO` for any field
   not yet available.
3. Commit on a branch in your fork and open a pull request against `main` of the
   upstream repository.

Step-by-step instructions, including the full entry template and both web-UI and
command-line flows, are in [`catalogue/README.md`](catalogue/README.md).

## Content Model Guidance

Each QTSP entry should be able to include:

- a unique id suitable for use in URLs;
- the QTSP name;
- a short description;
- one or more trust services or functionalities;
- a QTSP-level reference to the shared conformance or interoperability test
  results source;
- optional supporting details relevant for use case groups.

The model should support one QTSP having several listed services at the same
time.

Interoperability or conformance test results should not be duplicated under
individual attestations when a shared central source exists. In that case, the
catalogue should reference the central source once at QTSP level.
