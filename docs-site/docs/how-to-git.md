# Intro to Git & Workflows

This page walks through the steps for working on this repository with Git. The site is published from `main`; day-to-day work happens on `dev`.

To maintain control of changes @KevinHe and @JohnLe have been assigned as the reviewers of merge requests.

> All changes that are pushed to `dev` will not be viewable in the docs website or in Github until either Kevin or John approve.

## Our branches
Our Github repo has three branches, `main`, `dev` and `gh-pages`. The first two are the only ones for acutal work `gh-pages` is only for hosting our documents.

| Branch | Purpose | Who pushes |
| --- | --- | --- |
| `main` | Published site. Every merge deploys. | Nobody directly. Changes arrive by pull request from `dev`, approved by Kevin or John. |
| `dev` | Shared working branch where all documentation is added. | Everyone, straight to `dev` (no pull request needed). |
| `gh-pages` | Built website files, written by CI. | Nobody. Never edit or push to it. |

## 1. Install the tools

| Tool | Link |
| --- | --- |
| Git | [git-scm.com/downloads](https://git-scm.com/downloads) |
| Visual Studio Code | [code.visualstudio.com/download](https://code.visualstudio.com/download) |

Once Git is installed please open your `terminal` or `command line` to check that Git is installed. Copy the following, go to the terminal and press `Ctrl + Shift + V` and press enter:

```bash
git --version
```

Once confirmed, git needs to know who is using it, so use your Github account emails to register the following (once per computer):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
Now open VS code and confirm if you see the following to continue.
![VS Code window](Images/VS%20Code.png)

### 1.1 Logging into Github on VS Code
A nice to have in this is to log into your github account in VS code. Click the bottom left icon above settings and log into your github account.

## 2. Clone the repository
Once you reach this page, click on clone and enter the following link to clone the repostiory.
```http
https://github.com/INFS3605-WA-18A-1/CapstoneProject.git
```

## 3. Create the `dev` branch (once, by one team member)

`main` is the published branch, so every push to it deploys the site. `dev` is our working branch.

```bash
git checkout main
git pull origin main
git checkout -b dev          # create dev from main and switch to it
git push -u origin dev       # publish dev to GitHub and track it
```

Everyone else just fetches it:

```bash
git fetch origin
git checkout dev
```

## 4. Daily workflow
>All changes being made should be communicated in the group chat but in case something is being missed, make sure to frequently checkout before starting development.

Use the following commands for daily work:

```bash
git checkout dev
git pull origin dev          # get teammates' latest work

# ...edit files...

git status                   # see what changed
git add .                    # stage changes
git commit -m "Describe what you changed"
git push origin dev
```

!!! tip
    **Pull** before you start working and before you push. It avoids most merge conflicts.

If you are adding any documentation made by an agent, please use the open preview using either `Ctrl+K, V`. Or the following UI button.
![View MD](Images/View%20MD.png)

## 5. Publish to `main`
To push your work to main where our hosting services and documentation can update on them, follow the following steps.

1. Push your work to `dev`.
2. On GitHub, open a **Pull Request** from `dev` into `main`.
3. Wait for the **docs-test** check to pass, get a teammate to review, then merge.
4. Merging to `main` automatically deploys the site.



Never push to `gh-pages` by hand; CI owns that branch.

## Adding documentation (Lena, Sophia and the rest of the team)

You only ever work on `dev`. You never need to touch `main` or `gh-pages`.

1. Switch to `dev` and get the latest work:

    ```bash
    git checkout dev
    git pull origin dev
    ```

2. Add or edit your Markdown files under `docs-site/docs/`. For a **new page**, also add it to `nav:` in `docs-site/mkdocs.yml`, otherwise the build fails. Use lowercase kebab-case file names and start each page with one `# Title`.
3. For **images**, put them in `docs-site/docs/Images/` and link them relative to your page, e.g. `![Alt text](Images/my-image.png)`.
4. Check the build passes:

    ```bash
    cd docs-site
    mkdocs build --strict
    ```

5. Commit and push straight to `dev`:

    ```bash
    git add .
    git commit -m "Add <what you added>"
    git pull origin dev     # pick up anyone else's pushes first
    git push origin dev
    ```

6. When your work is ready to be published, tell Kevin or John. They open the pull request from `dev` into `main` and review it.

If `git push` is rejected, someone else pushed first. Run `git pull origin dev` and push again.

## Handy commands

| Command | What it does |
| --- | --- |
| `git branch` | List local branches (`*` marks the current one) |
| `git log --oneline` | Show recent commits |
| `git diff` | Show unstaged changes |
| `git restore <file>` | Discard unstaged changes to a file |
| `git stash` / `git stash pop` | Temporarily shelve and restore changes |
