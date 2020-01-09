git rebase
---

How to make rebasing less painful:

Providing you're working on git v1.7.3 or higher you can do

```bash
git rebase --strategy recursive --strategy-option theirs ${branch}
git rebase -X theirs
```

Amusingly the `-X` option for `rebase` assumes a recursive strategy.

Furthermore the `[theirs|ours]` means the opposite of a normal rebase
where

* `theirs` = the `"${branch}"` in question (opposite of normal)
* `ours` = the branch you're rebasing on-to (the currently checked out one).

(I think)
