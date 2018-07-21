---
layout: post
title: "git rebase --abort大灾难"
subtitle: "我以为我的代码找不回来了"
date: 2018-07-21 11:32:00
author: "marillo"
header-img: "img/about-bg.jpg"
catalog: true
tags:
    - Git
---

今天我照常提交代码，发现同事已经在同一分支提交了新的代码，我就`git pull --rebase`喽，结果提示我

```js
git push origin xxx git.example.com:app
 * branch            xxx    -> FETCH_HEAD

It seems that there is already a rebase-apply directory, and
I wonder if you are in the middle of another rebase.  If that is the
case, please try
        git rebase (--continue | --abort | --skip)
If that is not the case, please
        rm -fr "/Users/andrew/Development/sonar/.git/rebase-apply"
```

咦，怎么会报错呢，那我就先终止rebase吧，`git rebase --abort`来一发。

然后再`git pull --rebase`，嘻嘻，貌似一切正常嘛！我去看sourcetree吧。

不看不知道，一看吓一跳，我的commit log没有了。emmm.....

怎么办 ，内心千万只羊驼奔驰而过呀。

3秒钟冷静下来后，我就去万能的Google了。发现一位大哥和我一样，也是这种操作丢失代码了。我真是柳暗花明又一村阿。[解决方法](http://stackoverflow.com/a/2693668)

```js
$ git reflog | grep 50554 # 50554是commit信息的关键字
def8c08 HEAD@{5}: commit: FEEDBACK #50554
$ git reset --hard HEAD@{5}
HEAD is now at def8c08 FEEDBACK #50554
```

感谢强大的git reflow，感谢谷歌，感谢Stack Overflow。