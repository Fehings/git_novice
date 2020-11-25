teaching: 10
We've been adding one line at a time to `trepanation.txt`, so it's easy to track our
let's make a change to `trepanation.txt`, adding yet another line.
$ nano trepanation.txt
$ cat trepanation.txt
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
$ git diff HEAD trepanation.txt
diff --git a/trepanation.txt b/trepanation.txt
--- a/trepanation.txt
+++ b/trepanation.txt
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
$ git diff HEAD~1 trepanation.txt
$ git diff HEAD~3 trepanation.txt
$ git show HEAD~3 trepanation.txt
Author: User Name <user@newcastle.ac.uk>
    Start notes on trepanation
diff --git a/trepanation.txt b/trepanation.txt
+++ b/trepanation.txt
+Cheap and easy, just need a corkscrew.
$ git diff f22b25e trepanation.txt
diff --git a/trepanation.txt b/trepanation.txt
--- a/trepanationars.txt
+++ b/trepanation.txt
 Cheap and easy, just need a corkscrew.
+Kind of awful though, and rather old fashioned.
`trepanation.txt` (the "ill-considered change").
    modified:   trepanation.txt
$ git checkout HEAD trepanation.txt
$ cat trepanation.txt
Cheap and easy, just need a corkscrew.
Kind of awful though, and rather old fashioned.
$ git checkout f22b25e trepanation.txt
$ cat trepanation.txt
Cheap and easy, just need a corkscrew.
    modified:   trepanation.txt
$ git checkout HEAD trepanation.txt
> $ git checkout f22b25e trepanation.txt
> to revert `trepanation.txt` to its state after the commit `f22b25e`. But be careful! 
> if you forget `trepanation.txt` in the previous command.