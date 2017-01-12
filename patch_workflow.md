## 原则
* 一个 commit 一个 patch.

## steps
### gen patch file
* one patch file per commit: `git format-patch -1 <sha1>` or `HEAD`
* only one patch file: `git format-patch master(or other base branch) --stdout > [module_name]-[short_description].patch`

### apply patch file
* show stats: `git apply --stat file.patch`
* check for error before applying: `git apply --check file.patch`
* apply the patch finally: `git am < file.patch`

## If patch did not apply cleanly:
* `rejected-apply`
* `wiggle-rej`
* `git add -p`
* `git diff --cached`
* `git am --continue`
* `git show -1`
* `git pull && git push`
* `rej-clean`

## `rej-clean`

```
#!/bin/bash
find . -type f -name \*orig -print -delete
find . -type f -name \*rej -print -delete
```

## `rejected-apply`

```
#!/bin/bash
set -o nounset -o errexit
test -f .git/rebase-apply/patch

rej-clean
patch -p1 < .git/rebase-apply/patch
```

## `wiggle-rej`

```
#!/bin/bash
find . -name \*.rej |
while read rej
do
  echo $rej
  wiggle -r ${rej%.rej} $rej
done
```

## Refs
* <http://rypress.com/tutorials/git/patch-workflows>
* <https://www.oliverdavies.uk/blog/git-format-patch/>
* <https://www.drupal.org/node/1054616>
* <https://wiki.debian.org/ThomasKoch/GitPackagingWorkflow>