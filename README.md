# Git Workshop: Workflows
Branches, conflicts, and pull requests.

> [!IMPORTANT]
> **Instructors**: Before beginning, create a new workshop-specific repository by clicking the **Use this template** button on the [caltechlibrary/git-workshop-recipes](https://github.com/caltechlibrary/git-workshop-recipes) repository page.

## Table of Contents
1. [Setup](#setup)
1. [Branches](#branches)
1. [Conflicts](#conflicts)
1. [Pull Requests](#pull-requests)
1. [Acknowledgements](#acknowledgements)

## Setup

### Git Setup

Configure your global name and email.

```sh
git config --global user.name "Tommy Keswick"
git config --global user.email "415179+t4k@users.noreply.github.com"
```

Go to [GitHub Profile > Settings > Emails](https://github.com/settings/emails) to find your private email alias.

Create a local Git repository.

```sh
cd ~/Desktop
mkdir recipes
cd recipes
git init
touch guacamole.md
git add guacamole.md
git commit -m "begin first recipe"
```

### SSH Setup

Check if SSH keys have been set up before.

```sh
ls -al ~/.ssh
```

If not, create a new SSH key pair.

```sh
ssh-keygen -t ed25519 -C "tommy@macbook"
```

Access and copy the public key.

```sh
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub under [GitHub Profile > Settings > SSH and GPG keys](https://github.com/settings/keys).

Check that your machine can be authenticated by GitHub.

```sh
ssh -T git@github.com
```

## Branches

View the branches in the repository.

```sh
git branch
```

Work on the main branch.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
```

```sh
git commit -am "add guacamole ingredients"
```

Create a new branch.

```sh
git branch spicy
```

View the branches in the repository again.

```sh
git branch
```

Switch to our new branch.

```sh
git checkout spicy
```

See also [`git switch`](https://git-scm.com/docs/git-switch).

Note that we are now on our `spicy` branch.

```sh
git branch
# or
git status
```

Work on the `spicy` branch.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
- jalapeño
```

```sh
git commit -am "add some spice"
```

View our history on the `spicy` branch.

```sh
git log --oneline
```

Switch back to the `main` branch.

```sh
git switch main
# or `git checkout main` if that does not work
```

View our history on the `main` branch.

```sh
git log --oneline
```

Create and switch to a new branch.

```sh
git switch -c mild
# or `git checkout -b mild` if that does not work
```

Note that we are now on our `mild` branch.

```sh
git branch
# or `git status`
```

Work on the `mild` branch.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
- onion
```

```sh
git commit -am "add some flavor"
```

View our history on the `mild` branch.

```sh
git log --oneline
```

Switch back to the `main` branch.

```sh
git switch main
```

View our history on the `main` branch.

```sh
git log --oneline
```

Merge the changes from the `mild` branch into the `main` branch.

```sh
git merge mild
```

Delete the branch reference for the fast-forwarded branch.

```sh
git branch -d mild
```

Force delete the unmerged `spicy` branch, which deletes all work in the branch.

```sh
git branch -D spicy
```

## Conflicts

Create a new `instructions` branch from the current `main` branch.

```sh
git branch instructions
```

Continue working on `main` branch.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
- onion

# Instructions
TBD
```

```sh
git commit -am "add guacamole instructions placeholder"
```

Observe the `main` branch log.

```sh
git log --oneline
```

Switch to the `instructions` branch.

```sh
git switch instructions
# or `git checkout instructions` if that does not work
```

Work on the `instructions` branch.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
- onion

# Instructions
Go to the store.
```

```sh
git commit -am "begin guacamole instructions"
```

Observe the `instructions` branch log.

```sh
git log --oneline --graph --all
```

Switch to the `main` branch.

```sh
git switch main
# or `git checkout main` if that does not work
```

Merge the changes from the `instructions` branch into the `main` branch.

```sh
git merge instructions
```

Examine conflict.

```sh
cat guacamole.md
```

Edit the file to manually reconcile the changes.

```sh
nano guacamole.md
```

Add the resolved conflict changes and commit them to complete the merge.

```sh
git add guacamole.md
git status
git commit -m "resolve merge conflicts with instructions branch"
```

View the graph.

```sh
git log --oneline --graph --all
```

Switch to the `instructions` branch and merge with `main`.

```sh
git switch instructions
git merge main
git log --oneline --graph --all
```

Work on the `instructions` branch some more.

```sh
nano guacamole.md
```

```md
# Ingredients
- avocado
- lemon
- salt
- onion

# Instructions
1. Cut avocados.
2. Dice onion.
3. Squeeze lemon.
4. Pinch salt.
```

```sh
git commit -am "continue guacamole instructions"
```

Merge changes into main branch.

```sh
git switch main
git merge instructions
```

View current `guacamole.md` file.

```sh
cat guacamole.md
```

## Pull Requests

Fork the workshop repository provided by the instructor into your personal GitHub account.

Clone the forked repository to your local machine.

```sh
cd ~/Desktop
git clone git@github.com:t4k/WORKSHOP-REPOSITORY.git
```

Add an upstream remote to the local repository.

```sh
cd ~/Desktop/git-workshop-recipes-2024
git remote add upstream https://github.com/caltechlibrary/WORKSHOP-REPOSITORY.git
```

View the remotes for your local repository.

```sh
git remote -v
```

Instructor adds another recipe to the upstream repository.

Pull the latest content into your local repository and push to your remote fork.

```sh
git pull upstream main
git push origin main
```

Alternatively, use the **Sync fork** functionality from the GitHub interface.

Meanwhile, the upstream repository owner updates the README table of contents.

Everyone creates a local branch to add a new recipe.

```sh
git switch -c add-orangejuice
# or `git checkout -b add-orangejuice` if switch does not work
```

Confirm you are on the correct branch.

```sh
git branch
```

Create a new file for your recipe.

```sh
# optionally copy an existing recipe to modify
# `cp guacamole.md orangejuice.md`
nano orangejuice.md
```

Add and commit the changes.

```sh
git add orangejuice.md
git commit -m "add orange juice recipe"
```

Push changes to remote fork.

```sh
git push origin add-orangejuice
```

Use GitHub to initiate a pull request.

Upstream repository owner reviews pull request and makes suggestions.

Update pull request with suggested changes.

```sh
nano orangejuice.md
git commit -am "update orange juice recipe with suggestions"
git push origin add-orangejuice
```

Note that the pull request has been updated on GitHub.

Everyone also should add to the README table of contents within their pull request branch.

```sh
nano README.md
git commit -am "add orange juice to table of contents"
git push origin add-orangejuice
```

Note the conflict in the pull request.

Resolve the conflict manually in your local repository and push to the pull request branch.

```sh
git pull upstream main
nano README.md
git commit -am "resolve conflict in table of contents"
git push origin add-orangejuice
# or use the “Resolve conflicts” button in GitHub
```

## Acknowledgements

Content has been adapted from the following lessons.

- [swcarpentry/git-novice](https://github.com/swcarpentry/git-novice): Git configuration and SSH setup.
- [UCL/git-novice](https://github.com/UCL/git-novice): Recipe paradigm with guacamole starter.
- [carpentries-incubator/git-novice-branch-pr](https://github.com/carpentries-incubator/git-novice-branch-pr): Branching, conflicts, and pull requests steps.
