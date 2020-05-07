---
title: how to remove files from git without deleting them
source: https://stackoverflow.com/a/1143800/2037537
---

You added some files to your git repo, but now you want to ignore them.
So you add the files to `.gitignore`. But this does not remove them from 
the current repo.

The flag `--cached` is your friend

So, for a single file:

    git rm --cached mylogfile.log

and for a single directory:

    git rm --cached -r mydirectory
