---
name: dop-setup
description: Install or update Domain–Object–Process Programming rules in a project's agent instructions. Use when the user asks to adopt, set up, or refresh DOP; routine development follows the installed rules.
---

# DOP Setup

Install the complete rule block from [assets/DOP.md](assets/DOP.md) into the project's agent instructions. Setup changes instructions only; reorganizing source code is a separate task.

## Target

Use the project the user identifies, otherwise the current repository root or current directory if outside a repository. In a monorepo, install once at the workspace root; the rules apply to every package or crate.

Use the root `AGENTS.md`. If it does not exist and the project uses `CLAUDE.md`, use that file; otherwise create `AGENTS.md`. Follow an existing instruction-file symlink only when its target belongs to this project. Do not modify a global or externally shared instruction file as project setup.

Read the target file and the bundled `assets/DOP.md` before editing. The bundled file is the canonical rule block; copy it verbatim, including `<!-- DOP:START -->` and `<!-- DOP:END -->`. It contains everything agents need for ongoing development.

## Install or update

- **No DOP instructions:** append the complete block, separated from existing content by a blank line.
- **One marked DOP block:** replace that block. If it already matches, leave it unchanged.
- **An unmarked DOP section:** replace its DOP rules with the marked block. Preserve project-specific decisions outside the markers.

Preserve all unrelated instructions and project-specific additions. If additions appear inside an existing DOP block, move them immediately after the replacement block without changing their wording. Keep future project-specific additions outside the markers.

If the DOP section's boundaries or the distinction between shared rules and local additions cannot be established, leave the file unchanged and ask about the ambiguous text. Apply the same rule to incomplete or multiple marker pairs; do not guess what to overwrite.

Do not create source directories, move code, install dependencies, or add ongoing network lookups. Updating this skill supplies a new bundled rule block; running setup again applies it to the project.

## Verify

Read the result. Confirm there is exactly one complete DOP block matching the bundled file, that unrelated instructions and project-specific additions survive, and that source files are unchanged. Report the instruction-file path and whether the block was installed, updated, or already current. Mention any project-specific additions moved outside the markers.
