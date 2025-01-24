GerritHub Test Package
======================

## Contribute

This repository uses [GerritHub](https://gerrithub.io) as review platform,
which a hosted gerrit instance directly wired with GitHub and the whole
review and maintenance work is done on that platform.

### Checkout

#### Clone repository

**Clone from GerritHub (recommended)**

> **Be aware** to replace the `john-doe` gerrithub/github user name with your username.

```shell
git clone "ssh://john-doe@gerrithub.io:29418/sbuerk/gerrithub-demo-package" \
  && cd gerrithub-demo-package
```

**Clone from GitHub**

```shell
mkdir -p ${HOME}/projects \
  && git clone git@github.com:sbuerk/gerrithub-demo-package.git ${HOME}/projects/gerrithub-demo-package \
  && cd ${HOME}/projects/gerrithub-demo-package
```

#### Configure checkout repository

**Configure repository specific author information**

> Replace email and username with your values. Only needed when it differs from your global setting.

```shell
git config user.email "john.doe@example.com" \
  && git config user.name "John Doe" \
```

**Configure push url**

Push url is configured to ensure that all pushes are processed against the gerrit instance.

> **Be aware** to replace the user-name `john-doe` in the following command with your gerrithub/github username.

```shell
git config remote.origin.pushurl \
  ssh://john-doe@gerrithub.io:29418/sbuerk/gerrithub-demo-package
```

**Configure rebase strategy**

Gerrit based workflow emphasizes linear git history, which means no merge commits. To avoid accidentally getting merge
commits when rebaseing a branch to retrieve latest merged changes it is recommended to configure the `autosetuprebase`
to `remote`.

```shell
git config branch.autosetuprebase remote
```

**Install required commit hook (gerrit changeid)**

`commit-msg` hook is mandatory to ensure that a valid gerrit change id is added to the commit message. This repository
uses the extended version from [TYPO3](https://github.com/TYPO3/typo3/blob/main/Build/git-hooks/commit-msg) instead of
the original gerrit hook to have extended checks to ensure commit message requirements, adopting the rules from TYPO3.

```shell
cp Build/git-hooks/commit-msg .git/hooks/ \
  && chmod +x .git/hooks/commit-msg
```

**Simply pushing for a branch**

It is possible to configure `<local-branch> <--> <gerrit-branch>` mappings for simply pushing on branches,
for example pushing changes to gerrit remote branch you have to use following push command:

```shell
git push origin HEAD:refs/for/main
```

This can be simplified to automatically push to the configured remote branch for a branch,
see following synopsis

> SYNOPSIS: git config remote.origin.push +refs/heads/<local-branch>:refs/for/<remote-branch> 

```shell
git config remote.origin.push +refs/heads/main:refs/for/main
```

or some more examples:

```shell
git config remote.origin.push +refs/heads/1.x.x:refs/for/1.x.x
git config remote.origin.push +refs/heads/13.4:refs/for/13.4
git config remote.origin.push +refs/heads/dedicated-working-branch:refs/for/13.4
```
