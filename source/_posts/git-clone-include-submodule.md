---
title: git_clone_include_submodule
tags:
  - git
  - submodule
  - clone
categories: git
date: 2017-06-01 02:18:10
---

# git clone 指定分支，并 register and update submodules
If your submodule was added in a branch be sure to include it in your clone command...
```
git clone -b <branch_name> --recursive <remote> <directory>
```
>If `--recursive` is specified, this command will recurse into the registered submodules, and update any nested submodules within.




---
参考： [How to \`git clone\` including submodules?](https://stackoverflow.com/questions/3796927/how-to-git-clone-including-submodules)
