# Git Work Flow for Github

## Motivation

Sometimes we will find that we need to execute lots of git command to make a pull request. For example, if we need to make a pull request to merge feature/fa branch, we need to do:

```shell
# remove deleted origin branches
git fetch -p origin

# update main branch to avoid potential conflicts
git switch main
git pull --rebase origin main

# update the current feature branch to avoid potential conflicts
git switch feature/fa
git rebase main

# create pr
gh pr create -f --base main

# show current branch
git branch
```

These steps are tedious. It's better that we can wrap these steps to one single command, and that is exactly what github-git-workflow does!

## Install

github-git-workflow uses `script-launcher` and github command cli `gh`, so you need to install `gh` firstly and then `script-launcher`.

Firstly, install [gh](https://github.com/cli/cli), and make sure you have [logged in](https://cli.github.com/manual/).

Secondly, install script-launcher.

```
npm install script-launcher -D
```

Then add `start: launch` to the `package.json` scripts section:

```
{
    ...
    "scripts": {
        "start": "launch",
        ...
    },
    ...
}
```

Then copy `launcher-config.json` to your project dir, in the same dir as your `package.json` file.

## Usage

Github-git-workflow uses git topic branch, feature/_ branches for adding new features and hotfix/_ branches for fixing issues directly from main branch.

The life cyle of branches are `create, finish and delete`.

**create**: create the topic branch

**finish**: create the pull request from the topic branch

**delete**: after pull request is merged to main branch, delete the local topic branch

## Feature Branch

### Create

```
feature:branch-name
```

examples:

```
npm start feature:doc
```
