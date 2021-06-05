---
created: 2021-06-06T01:14:44 (UTC +08:00)
tags: []
source: http://www.howardism.org/Technical/WeeklyTips/zsh-word-substitution.html
author: 
---

# Howardism: zsh: Bang History Word Substitution

> ## Excerpt
> Yes, your job is repetitive, and typing a series of git add, 
git commit and git push doesn't help.

---
Yes, your job is repetitive, and typing a series of `git add`, `git commit` and `git push` doesn't help.

When I need to _redo_ something I've already done, usually I just hit the up-arrow key if the shell command was recent (like one or two commands ago), or type `Control-R` and followed by some keyword on the command line. Sure, this approach often requires multiple `Control-R` keystrokes until I would get to the right command, but this feature has served me well for thousands of years.

This is why I never bothered with the _bang history substitution_ approach, e.g. `!43` to repeat the 43rd command entry. But I just learned about _word substitutions_ while perusing the `zshexpn` man page.

Let's assume you've done the following commands:

```
$ git add src/main/resources/nav/unified/templates/nav.handlebars
$ git commit -m "Fixed bug #249, and well, you know. . ."
$ git push origin HEAD:refs/publish/development
```

Now, with the next bug on your queue, you want to edit that same file, but copying/pasting is so 1984. Type this:

```
$ echo !-4:2
```

You can now either hit `Return` or `Tab`, and that filename shows up after the `echo` command. Pretty slick.

How does it work? Well the `-4` says, grab the 4th previous entry††Yes, it is the fourth since what you are typing now is always `-1` . The `:2` part says, grab the second argument to the command, in this case, the filename.

But who wants to count the history? Try this on for size:

```
$ echo !git:3
```

This echoes the third argument to the last `git` command you typed, in my particular example, that would be the remote branch name.

But how to get that filename four lines back without counting? How about:

```
$ echo !?add?:$
```

This looks through the history until it finds an `add` parameter (in this case, attached to the `git` command), and then brings back the final argument (that's what the `:$` tells it).

Here is a snippet from the Zshell man page (`zshexpn`) for all of the _Word Designators_:

-   `0` The first input word (command).
-   `n` The nth argument.
-   `ˆ` The first argument. That is, 1.
-   `$` The last argument.
-   `%` The word matched by (the most recent) `?str` search.
-   `x−y` A range of words; `x` defaults to `0`.
-   `*` All the arguments, or a null value if there are none.
-   `x*` Abbreviates `x−$`.
-   `x−` Like `x*` but omitting word `$`.

> Note that a `%` word designator works only when used in one of `!%`, `!:%` or `!?str?:%`, and only when used after a `!?` expansion (possibly in an earlier command). Anything else results in an error, although the error may not be the most obvious one.

Tell others about this article:

 [![Click here to submit this page to Stumble It](http://www.stumbleupon.com/images/16x16_su_3d.gif)](http://www.stumbleupon.com/submit?url=http://www.howardism.org/Technical%2FWeeklyTips%2Fzsh-word-substitution.html&title=zsh%3A%20Bang%20History%20Word%20Substitution)
