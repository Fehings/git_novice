---
title: Exploring History
teaching: 10
exercises: 0
questions:
- "How can I identify old versions of files?"
- "How do I review my changes?"
- "How can I recover old versions of files?"
objectives:
- "Explain what the HEAD of a repository is and how to use it."
- "Identify and use Git commit numbers."
- "Compare various versions of tracked files."
- "Restore old versions of files."
keypoints:
- "`git diff` displays differences between commits."
- "`git checkout` recovers old versions of files."
---

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `trepanation.txt`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `trepanation.txt`, adding yet another line.

~~~
$ nano trepanation.txt
$ cat trepanation.txt
~~~
{: .language-bash}

~~~
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
An ill-considered change
~~~
{: .output}

Now, let's see what we get.

~~~
$ git diff HEAD trepanation.txt
~~~
{: .language-bash}

~~~
diff --git a/trepanation.txt b/trepanation.txt
index b36abfd..0848c8d 100644
--- a/trepanation.txt
+++ b/trepanation.txt
@@ -1,3 +1,4 @@
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
+An ill-considered change.
~~~
{: .output}

which is the same as what you would get if you leave out `HEAD` (try it).  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1` 
(where "~" is "tilde", pronounced [**til**-d*uh*]) 
to refer to the commit one before `HEAD`.

~~~
$ git diff HEAD~1 trepanation.txt
~~~
{: .language-bash}

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:


~~~
$ git diff HEAD~3 trepanation.txt
~~~
{: .language-bash}


We could also use `git show` which shows us what changes we made at an older commit as 
well as the commit message, rather than the _differences_ between a commit and our 
working directory that we see by using `git diff`.

~~~
$ git show HEAD~3 trepanation.txt
~~~
{: .language-bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: User Name <user@newcastle.ac.uk>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on trepanation

diff --git a/trepanation.txt b/trepanation.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/trepanation.txt
@@ -0,0 +1 @@
+Cheap and easy, just need a corkscrew.
~~~
{: .output}

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.

We can also refer to commits using
those long strings of digits and letters
that `git log` displays.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any computer
has a unique 40-character identifier.
Our first commit was given the ID
`f22b25e3233b4645dabd0d81e651fe074bd8e73b`,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters (typically seven for normal size projects):

~~~
$ git diff f22b25e trepanation.txt
~~~
{: .language-bash}

~~~
diff --git a/trepanation.txt b/trepanation.txt
index df0654a..93a3e13 100644
--- a/trepanationars.txt
+++ b/trepanation.txt
@@ -1 +1,4 @@
 Cheap and easy, just need a corkscrew.
+Kind of awful though, and rather old fashioned.
+An ill-considered change
~~~
{: .output}

All right! So
we can save changes to files and see what we've changed. Now, how
can we restore older versions of things?
Let's suppose we change our mind about the last update to
`trepanation.txt` (the "ill-considered change").

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   trepanation.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

We can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD trepanation.txt
$ cat trepanation.txt
~~~
{: .language-bash}

~~~
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
~~~
{: .output}

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead:

~~~
$ git checkout f22b25e trepanation.txt
~~~
{: .language-bash}

~~~
$ cat trepanation.txt
~~~
{: .language-bash}

~~~
Cheap and easy, just need a corkscrew.
~~~
{: .output}

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   trepanation.txt

~~~
{: .output}

Notice that the changes are currently in the staging area.
Again, we can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD trepanation.txt
~~~
{: .language-bash}

> ## Don't Lose Your HEAD
>
> Above we used
>
> ~~~
> $ git checkout f22b25e trepanation.txt
> ~~~
> {: .language-bash}
>
> to revert `trepanation.txt` to its state after the commit `f22b25e`. But be careful! 
> The command `checkout` has other important functionalities and Git will misunderstand
> your intentions if you are not accurate with the typing. For example, 
> if you forget `trepanation.txt` in the previous command.
>
> ~~~
> $ git checkout f22b25e
> ~~~
> {: .language-bash}
> ~~~
> Note: checking out 'f22b25e'.
>
> You are in 'detached HEAD' state. You can look around, make experimental
> changes and commit them, and you can discard any commits you make in this
> state without impacting any branches by performing another checkout.
>
> If you want to create a new branch to retain commits you create, you may
> do so (now or later) by using -b with the checkout command again. Example:
>
>  git checkout -b <new-branch-name>
>
> HEAD is now at f22b25e Start notes on Mars as a base
> ~~~
> {: .error}
>
> The "detached HEAD" is like "look, but don't touch" here,
> so you shouldn't make any changes in this state.
> After investigating your repo's past state, reattach your `HEAD` with `git checkout master`.
{: .callout}

It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to discard.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`:

![Git Checkout](../fig/git-checkout.svg)

So, to put it all together,
here's how Git works in cartoon form:

![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](../fig/git_staging.svg)

> ## Simplifying the Common Case
>
> If you read the output of `git status` carefully,
> you'll see that it includes this hint:
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {: .language-bash}
>
> As it says,
> `git checkout` without a version identifier restores files to the state saved in `HEAD`.
> The double dash `--` is needed to separate the names of the files being recovered
> from the command itself:
> without it,
> Git would try to use the name of the file as the commit identifier.
{: .callout}

The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.

