# GitHub repository with Submodule

## Introduction

This repository contains multiple submodules, which are external Git repositories embedded within this project. These submodules allow us to maintain dependencies or shared codebases independently while integrating them seamlessly into the main project.

Each submodule has its own version control, and changes within a submodule must be handled separately.

---

## Getting Started

### Prerequisites
Make sure you have the following installed:
- [Git](https://git-scm.com/) version 2.11 or higher
- [GitHub CLI](https://cli.github.com/) (optional, but recommended)

### Cloning the Repository

When cloning a repository with submodules, you must use the `--recurse-submodules` flag to ensure all submodules are initialized and cloned automatically.

```bash
git clone --recurse-submodules <repository_url>
```

Alternatively, if you've already cloned the repository without submodules, you can initialize them afterward by running:

```bash
git submodule update --init --recursive
```

### Initializing Submodules

If you cloned the repository without initializing submodules, run the following commands to get them set up:

```bash
git submodule init
git submodule update
```

This will initialize and fetch all submodules as defined in the `.gitmodules` file.

---

## Working with Submodules

### Updating Submodules

To ensure your submodules are up-to-date, use the following command:

```bash
git submodule update --remote --merge
```

This will update the submodules to the latest commit on the specified branch.

### Adding New Submodules

To add a new submodule to the repository, follow these steps:

1. Add the submodule using the following command:

   ```bash
   git submodule add <repository_url> <path_to_submodule>
   ```

   Example:

   ```bash
   git submodule add https://github.com/user/submodule.git modules/submodule
   ```

2. Commit the changes to include the submodule:

   ```bash
   git commit -m "Added new submodule"
   ```

3. Push the changes to the remote repository:

   ```bash
   git push origin main
   ```

### Removing Submodules

To remove a submodule, follow these steps:

1. Remove the submodule entry from the `.gitmodules` file.

2. Remove the submodule from the Git index:

   ```bash
   git rm --cached <path_to_submodule>
   ```

3. Delete the submodule from the file system:

   ```bash
   rm -rf <path_to_submodule>
   ```

4. Remove the submodule's configuration from `.git/config`.

5. Commit and push the changes:

   ```bash
   git commit -m "Removed submodule"
   git push origin main
   ```

---

## Pushing Changes

If you make changes to a submodule, you need to commit those changes within the submodule and push them to the submodule's repository.

1. Navigate to the submodule's directory:

   ```bash
   cd <path_to_submodule>
   ```

2. Stage, commit, and push your changes:

   ```bash
   git add .
   git commit -m "Updated submodule"
   git push origin main
   ```

3. After committing and pushing the changes to the submodule, return to the main repository:

   ```bash
   cd ..
   ```

4. Stage and commit the submodule's updated state in the main repository:

   ```bash
   git add <path_to_submodule>
   git commit -m "Updated submodule pointer"
   git push origin main
   ```

---