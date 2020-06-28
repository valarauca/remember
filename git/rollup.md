Roll Up / Squash Commits
---

How to Squash Commits

### Setting your editor

Tell git your editor of choice

```bash
git config --global core.editor vim
```

### Set up interactive rebase

You need to tell git which commits it needs to work with.

The one method is to just say a number of commits, for example `9`

```bash
git rebase -i HEAD~9
```

The other method is to just use a commit, and rebase everything after,
for ideal function this should be the current `master` your branch/fork
occured _from_.

```bash
git rebase -i ${EVERYTHING_AFTER_SHA}
```

You will then see something like

```
0 pick 60042c8 who knows
1 pick 83c9bbb foo
2 pick 14a64ff bar
3 pick c289adc why won't this work
4 pick 97178bd adfadfasdf
5 pick 76e5811 ugh
6 pick a135e94 one more thing
7 pick 1bd999c forgot about that
8 pick 44d2565 review comments
```

From the `vim` session make this look like

```
0 p 60042c8 who knows
1 s 83c9bbb foo
2 s 14a64ff bar
3 s c289adc why won't this work
4 s 97178bd adfadfasdf
5 s 76e5811 ugh
6 s a135e94 one more thing
7 s 1bd999c forgot about that
8 s 44d2565 review comments
```

Give it that `ESC + :wq` love, and hopefully you'll get a non-error message.

If you get an error message, play around with the range, IDFK
