# git cleanup-branches

When you're working on some project for a while the amount of git branches is
getting up, so it's getting hard to understand which branches are still useful
and which were already merged and deleted from the remote. This script tries to
address this problem by providing a simple, interactive experience to remove branches
which were deleted at remote and those which were already merged.

This simple script will delete local branches which are:

- deleted in remote (remote name could be set via `--remote`, by default `origin`)
- merged into the current branch

The script is interactive and will ask before pruning the remote or deleting
branches.

**Note:** you need python3 to be installed to run this script

Installation:

```
curl https://raw.githubusercontent.com/weirdgiraffe/git-cleanup-branches/master/git-cleanup-branches.py --output /usr/local/bin/git-cleanup-branches
chmod +x /usr/local/bin/git-cleanup-branches
```
Then open a new shell, go to a git repository and use it with `git cleanup-branches`.

# examples

The script gives you a list of branches to be deleted and waits for confirmation:

```
$ git cleanup-branches
  prune: feature/a was deleted in origin. delete? (y/N): n
  prune: feature/b was deleted in origin. delete? (y/N): y
 merged: feature/c was already merged to master. delete? (y/N): n

local branches to keep:
- feature/a
- feature/c

local branches to delete:
- prune feature/b (prune origin)

if you will delete those branches you will
not be able to restore them from remote.
continue? (y/N): n
```

Script will warn you in case you want to keep local branches which were deleted
from remote:

```
$ git cleanup-branches
  prune: feature/a was deleted in origin. delete? (y/N): n
  prune: feature/b was deleted in origin. delete? (y/N): y
 merged: feature/c was already merged to master. delete? (y/N): y

local branches to keep:
- feature/a

local branches to delete:
- prune feature/b (prune origin)
- prune feature/c (merged into master)

if you will delete those branches you will
not be able to restore them from remote.
continue? (y/N): y
Deleted branch feature/b (was 031b5b312).
Deleted branch feature/c (was ab120153e).

stale branches in origin are about to be pruned.
if you will press y now it would be impossible
to link your stale local branches to stale
remote branches in origin.

prune remote origin? (y/N): y
Pruning origin
URL: git@github.com:xxx.git
 * [pruned] origin/feature/a
 * [pruned] origin/feature/b
```

You can skip interactive selection by running with `--all`:

```
$ git cleanup-branches --all

local branches to delete:
- prune feature/b (prune origin)
- prune feature/b (prune origin)
- prune feature/c (merged into master)

if you will delete those branches you will
not be able to restore them from remote.
continue? (y/N): y
Deleted branch feature/a (was ce195b1ef).
Deleted branch feature/b (was 031b5b312).
Deleted branch feature/c (was ab120153e).
Pruning origin
URL: git@github.com:xxx.git
 * [pruned] origin/feature/a
 * [pruned] origin/feature/b
```

You can specify remote name (`origin` by default):

```
$ git cleanup-branches --remote foo --all

local branches to delete:
- prune feature/b (prune foo)
- prune feature/b (prune foo)
- prune feature/c (merged into master)

if you will delete those branches you will
not be able to restore them from remote.
continue? (y/N): y
Deleted branch feature/a (was ce195b1ef).
Deleted branch feature/b (was 031b5b312).
Deleted branch feature/c (was ab120153e).
Pruning foo
URL: git@github.com:xxx.git
 * [pruned] foo/feature/a
 * [pruned] foo/feature/b
```
