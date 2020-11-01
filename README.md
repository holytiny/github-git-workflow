# Git Work Flow for Github

- [Motivation](#motivation)
- [Install](#install)
- [Usage](#usage)
- [Feature Branch](#feature-branch)
  - [Create](#create)
  - [Finish](#finish)
  - [Delete](#delete)

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

```shell
npm install script-launcher -D
```

Then add `start: launch` to the `package.json` scripts section:

```json
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

**finish**: create the pull request from the topic branch, use this command to avoid potential conflicts

**verify**: if something wrong for pull request, after serveral commits you may fix the issue, then use verify to push local fix to remote topic branch to avoid potential conflicts

**delete**: after pull request is merged to main branch, delete the local topic branch

## Feature Branch

### Create

```shell
feature:name
```

Create a feature topic branch named doc

```shell
npm start feature:doc
```

Then a new branch named `doc` should be created.

```shell
* feature/doc
  main
```

### Finish

```shell
finish-feature:name
# or for short
ff:name
```

After all development and test, we can use finish command to creat the pull request.

```shell
npm start finish-feature:doc
# or for short
npm start ff:doc
```

The command should create a pull request.

### Verify

```shell
verify-feature:name
# or for short
vf:name
```

If there's someting wrong with the pull request, and we fix the problem in local branch after several commits, then we can use verify command to push these commits to the remote branch of this pull request.

```shell
npm start verify-feature:doc
# or for short
npm start vf:doc
```

### Delete

```shell
delete-feature:name
# or for short
df:name
```

After the pull request is approved, we can use delete-feature command to delete the topic branch.

```shell
npm start delete-feature:doc
# or for short
npm start df:doc
```
