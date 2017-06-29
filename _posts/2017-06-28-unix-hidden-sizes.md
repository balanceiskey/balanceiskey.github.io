---
layout: post
title: Hunting down hidden file and folder sizes in Unix environments
---

Ever need to display the file or folder sizes for a given directory? That's super easy in Unix environments:

```bash
du -sh *
```

Check out the `du` man page for more. The standard `du` command listed above doesn't display quite everything as it leaves out hidden files. To get that, you'll need something like this:

```bash
du -sh .[^.]*
```

What is `.[^.]*`? I have no idea. The [Stack Overflow](https://askubuntu.com/posts/356902/revisions) I dug up explains it this way:

> @Dr_Bunsen: It's a glob that lists all the files that start with a single .. Here's a neat trick: if you don't know what a glob-looking thing does, try running echo .[!.]* or whatever. The shell will then expand the glob and pass it into echo, printing out the list of files that results.

The same article also mentions `ncdu`, an ncurses-loaded version of `du` and I must say, it is awesome.

![ncdu](/img/ncdu.gif)
