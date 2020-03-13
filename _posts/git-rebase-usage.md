---
title: Git Rebase：Git中的后悔药
date: 2017-08-28 13:35:13
tags: git
---
如果你没有睡醒，或是没有吃饱，那么Commit的时候你可能会写错别字。

别笑，我刚就写错了。

为了避免Push到仓库被人笑话的尴尬，要想办法修改Commit才行。

```
git commit --amend
```

成功回退到上次Commit的界面。

但是，在提交完这次Commit后，我才发现上次Commit的错误，`--amend`只能回退一次，并不能解决问题。

那么后悔药Rebase来了。

```
git rebase -i HEAD~2
```

进入到了Vim的界面，修改错别字那个commit前面的“pick”为“edit”吧，保存。

这时时光倒流……呸，这时HEAD被设置到了你错误的commit的那次提交。

```
git commit --amend
```

YES，这就是那个尴尬的提交，赶紧更正错别字，保存。

好了，错别字是更正了，但是怎么回去，不能丢掉最后一次的Commit呀。

```
git rebase --continue
```

Push，搞定。

*由于git的数据结构的特性，从被修改的提交开始到后面的所有提交的hash全部会发生变化。*

*如果在这之前你已经Push了，此时必须使用force-push推送你的更正后的版本避免冲突。（如果有人在你更正前pull过，他push时就会再次发生冲突，所以建议在commit的时候谨慎操作，push后尽量避免force-push）*