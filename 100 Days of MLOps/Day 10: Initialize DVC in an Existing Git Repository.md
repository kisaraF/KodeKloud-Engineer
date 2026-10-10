# Day 10: Initialize DVC in an Existing Git Repository

## Task Description

The xFusionCorp Industries ML team is implementing DVC to ensure that datasets and model files are versioned independently from the codebase. Initialize DVC within the existing Git repository located at `/root/code/fraud-detection/` and record this initialization in Git.

A Git repository already exists at `/root/code/fraud-detection/` with an initial commit.

Acceptance criteria:

- DVC is initialised inside that repository, so the standard `.dvc/` control directory and `.dvcignore` file exist alongside the existing Git working tree.
- Every file DVC produces during initialisation is recorded in a new Git commit with the message `Initialize DVC`.

> Once initialisation is complete, the DVC extension will detect the new .dvc/ directory and surface the DVC TRACKED section in the EXPLORER panel together with a DVC indicator in the bottom status bar.

## Solution

**Step 01: Initialize DVC**

dvc is already installed system-wide in this codespace. Therefore doing the below is enough

```bash
dvc init
```

**Step 02: Add to git**

```bash
git add . && git commit -m "Initialize DVC"
```

## References

- Read further about data version control from [this](https://www.datacamp.com/tutorial/data-version-control-dvc) post.
- [Offical DVC site](https://dvc.org/)