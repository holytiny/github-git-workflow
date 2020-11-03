# Git Work Flow for Github

- [Motivation](#motivation)
- [Install](#install)
- [Usage](#usage)
- [Feature Branch](#feature-branch)
- [Hotfix Branch](#hotfix-branch)
- [Version](#version)

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

**version**: version command is used in main branch, after the pull request is approved, version command can be used to add version tag to the main branch.

**delete**: after pull request is merged to main branch, delete the local topic branch

## Feature Branch

Feature branch is operated using rebase as much as possible to solve potential conflicts in local topic branch before the pull request.

### Create

```shell
feature:name
```

Create a feature topic branch named doc

```shell
npm start feature:doc
```

Then a new branch named `feature/doc` should be created.

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

## Hotfix Branch

Hotfix branch is used to fix issues in released version. It doesn't use rebase, conflicts are solved in pull request.

### Create

```shell
hotfix:name:version
```

This command create a branch named `hotfix/name` from the specific `version` tag, the `version` is a git version tag.

```shell
npm start hotfix:doc:v1.0.1
```

After the command, a branch named hotfix/doc is created.

```shell
* hotfix/doc
  main
```

### Finish

```shell
finish-hotfix:name
# or fh for short
fh:name
```

This command create a pull request from the topic branch.

```shell
npm start fh:doc
```

### Verify

```shell
verify-hotfix:name
# or vf for short
vf:name
```

The same as verify-feature, however, it doesn't rebase the topic branch from main branch.

```shell
npm start vf:doc
```

### Delete

```shell
delete-hotfix:name
# or dh for short
dh:name
```

The same as delete-feature, to delete the hotfix branch after the pull request is approved.

```shell
npm start dh:doc
```

## Version

Version command uses npm version to add tag go main branch.

If the version before executing the command is v1.0.0

```shell
npm start prepatch
# v1.0.1-0
```

```shell
npm start patch
# v1.0.1
```

```shell
npm start preminor
# v1.1.1-0
```

```shell
npm start minor
# v1.1.1
```

```shell
npm start premajor
# v2.1.1-0
# The recommand way is to use alpha or beta command rather than premajor
```

```shell
npm start major
# v2.1.1
```

```shell
npm start alpha
# v2.1.1-alpha.0
```

```shell
npm start beta
# v2.1.1-beta.0
```

```shell
# or specify the preid
npm start prerelease:omega
# v2.1.1-omega.0
```

```shell
# or specify the version
# npm start version:semver
npm start version:2.2.0
# v2.2.0
```
