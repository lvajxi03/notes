# Git

* 1

```sh
$ git rev-list [ commit-sha-id ]
```

* 2

Pushing empty commits to remote:

```sh
$ git commit --allow-empty
```

* 3

```sh
$ git commit --amend
$ git push -f
```

* 4

```sh
$ git tag -d 12345
$ git push origin :refs/tags/12345
```

* 5

```sh
$ git branch –D branch name (delete from local)
$ git push origin :branch name (delete from stash)
```

As of Git v1.7.0, you can delete a remote branch using

```sh
$ git push origin --delete <branchName>
```

which is easier to remember than

```sh
$ git push origin :<branchName>
```

which was added in Git v1.5.0 "to delete a remote branch or a tag."

* 6
```sh
$ git log --no-walk --tags --pretty=format:"%h %d %an" --decorate=full
```

* 7
Checkout/create branch from given tag:

```sh
$ git checkout -b newbranch v1.0
```

v1.0 -- tag name

newbranch -- branch name

## Useful tricks

```sh
alias gl='git log --graph --all --pretty=format:'\''%C(auto,yellow)commit %H%C(auto,green bold)%d%Creset%nAuthor: %an%nDate: %ad%n%n%s%n'\'''
```
