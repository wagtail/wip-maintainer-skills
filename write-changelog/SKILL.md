---
name: write-changelog
description: Write a Wagtail CHANGELOG.txt entry and matching release notes for a merged/about-to-merge change. This is a MAINTAINER task done at merge time — not something contributors do. Use when explicitly asked to add a changelog/release-note entry, or when committing a PR to main as a maintainer. Do NOT proactively add entries while reviewing contributor PRs.
---

# Writing changelog & release notes (Wagtail)

In Wagtail, **changelog and release-note entries are added by maintainers at merge time**, not by contributors (they're prone to git conflicts). Only do this when explicitly asked, or when committing an accepted change to `main` as a maintainer. Source of truth: [docs/contributing/committing.md](../../../docs/contributing/committing.md).

## 0. Determine the target release

**Before editing anything, confirm which release this change is expected to land in.** If the maintainer hasn't stated it, ask them — e.g. "Which release should this land in (the next feature release, or a patch like 7.4.2)?" Don't assume: a bug fix often goes to a patch release (e.g. `7.4.2`) while a feature goes to the in-development feature release (e.g. `8.0`). The target decides which `CHANGELOG.txt` section and which `docs/releases/X.Y.Z.md` file you touch.

## Files to update

1. **`CHANGELOG.txt`** — one **single line** per change, under the target release's `X.Y (xx.xx.xxxx) - IN DEVELOPMENT` section.
2. **`docs/releases/X.Y.Z.md`** — the release notes for the same version.
3. **`docs/releases/index.rst`** — make sure the release notes file is listed in the `toctree` (see below). Existing releases are already listed; a brand-new release file must be added.

## Categories & ordering

Within the section, entries are grouped in this order, identified by prefix:

| Group | Prefix | What it covers |
|---|---|---|
| Major features | *(none)* | Things that inspire users to upgrade |
| Minor enhancements | *(none)* | Other dev/end-user improvements |
| Bug fixes | `Fix:` | Fixes broken behavior from previous releases |
| Documentation | `Docs:` | Doc changes not tied to a specific code change |
| Maintenance | `Maintenance:` | Cleanup/refactoring/tooling, no dev/end-user impact |

End each line with the contributor's name(s) in brackets. Example:

```text
* Fix: Tags added on the multiple image uploader are now saved correctly (Alex Smith)
```

When multiple people contributed (e.g. original author + reviewers who pushed fixes), list them comma-separated: `(Jane Doe, Alex Smith)`.

## Release notes (`docs/releases/X.Y.md`)

- **Major features** get their own heading with a more detailed description.
- **Minor enhancements** go under an "Other features" heading; **bug fixes / docs / maintenance** are bullet points under their respective headings.
- The bullets are the changelog lines **with the prefix removed** (`Fix:`/`Docs:`/`Maintenance:`).
- Add **backwards-incompatibility notes** where relevant — see prior release notes for the format.

## List the file in `docs/releases/index.rst`

The release notes index is a `toctree` of version files (filenames **without** the `.md`/`.rst` extension), ordered **newest first**. An existing release will already be listed. If you created a new release notes file (see below), add its name to the toctree in the correct descending position — e.g. `7.4.3` goes immediately above `7.4.2`:

```rst
.. toctree::
   :maxdepth: 1

   upgrading
   release_process
   8.0
   7.4.3   <- new entry
   7.4.2
   ...
```

## Creating a new release notes file (if it doesn't exist yet)

If the target release has no `CHANGELOG.txt` section or `docs/releases/X.Y.Z.md` file yet, create them from these templates (example for `7.4.2`).

Add the new section to the **top** of `CHANGELOG.txt`:

```text
7.4.2 (xx.xx.xxxx) - IN DEVELOPMENT
~~~~~~~~~~~~~~~~

 * ...
```

Create `docs/releases/7.4.2.md`:

````md
# Wagtail 7.4.2 release notes - IN DEVELOPMENT

_Unreleased_

```{contents}
---
local:
depth: 1
---
```

## What's new

### Bug fixes

 * ...

### Documentation

 * ...

### Maintenance

 * ...
````

Then add `7.4.2` to the `docs/releases/index.rst` toctree as described above.

## First-time contributors

If it's the contributor's first contribution, add them to `CONTRIBUTORS.md` — in chronological order, at the **bottom** of the list, using their preferred name (check their GitHub profile; ask if unsure).

## Committing the entry

- If the change is a single commit, fold the entry into it:
  ```sh
  git add CHANGELOG.txt docs/releases/X.Y.Z.md docs/releases/index.rst CONTRIBUTORS.md
  git commit --amend --no-edit
  ```
- If it doesn't fit in one commit, make a separate commit messaged `Release notes for #xxxx`:
  ```sh
  git add CHANGELOG.txt docs/releases/X.Y.Z.md docs/releases/index.rst CONTRIBUTORS.md
  git commit -m 'Release notes for #xxxx'
  ```

## Judgement

Only **significant** changes need an entry. Trivial fixes (typos, a test-only fix, internal-only tweaks with no dev/end-user impact) typically don't — confirm with the maintainer rather than adding one by default.
