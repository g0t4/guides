# fdupes - Find duplicate files

## Links

- [fdupes](https://github.com/adrianlopezroche/fdupes)

## Description

This is a command line tool to find duplicate files. For example, if you regularly make a full backup of your files and need to go back and review for duplicates across multiple backups. This is the tool to use. Highly flexible so you're not spending hours clicking confirmations and still have full control over what will be removed and kept. As well as confidence you're not nuking important files.

## Install

```bash
brew install fdupes

# windows (WSL)
sudo apt install -y fdupes # can be older version b/c debian

# windows jdupes (TODO test this)
choco install jdupes # fork of fdupes

```

## Commands

````bash

fdupes dir1 # do not check nested directories
fdupes -r dir1 dir2 dir3

# TODO cache
fdupes --cache dir1
fdupes -x cache.prune
fdupes -x cache.clear

# quick check
fdupes -q dir1 dir2 dir3

man fdupes
# also type `help` in interactive mode (shows commands to select/mark files)

# * list only (not --delete)
--omitfirst # skips first match, thus only shows dups (i.e. confirm no dups left, or see how many)
--sameline/-1 # all dups listed on same line (i.e. another way to see how many dups)

# * summarize
fdupes --recurse . --order name --summarize
# 13 duplicate files (in 8 sets), occupying 589.8 megabytes

# * delete w/ confirmation (prompts) 
fdupes --delete ... # full screen, interactive prompt
fdupes --delete --plain ... # line-based prompts

# * delete w/o confirmation
fdupes --delete --noprompt ... # after scan completes
fdupes --delete --immediate ... # implies --noprompt, delete as discovered (during scan)

# * faster search
fdupes --deferconfirmation ... # do time/size checks for scan, then confirm byte for byte same files when deleting
fdupes --minsize=100K --maxsize=1M ... # filter by size (min, max, both) 

# filters
fdupes --nohidden # i.e. DS_Store (macOS), Thumbs.db (Windows)
fdupes --noempty # skip empty files

# extra fields:
--size # per duplicate group
--time # of each file

# sorting
fdupes --order name|time|ctime ... # IMO superior to just use screen prompts and select commands, not need to rely on sorting results

## Full Screen Delete Prompts

```bash

# type commands, i.e. 
help <ENTER>
quit # to exit (also `q` alone is enough)
yes # to exit (also `y` alone is enough)

# instead of one by one marking files, use search to select a subset and then tag them all at once, i.e.:
sel backups2 # selects all files in backups2 dir
selr \.mkv$ # regex selection, in this cse for files ending in .mkv
dsel/dselr # deselect matching files
csel # clear selections, otherwise new selections add or modify existing
# * read FILE SELECTION COMMANDS and TAGGING SELECTED FILES for many more commands

# once selected, mark files:
ks # mark keep
sel backups1 #
ds # mark delete
rs # remove tags from selected files

# page up/down to confirm
# when ready to apply changes:
<DELETE> # press delete key to nuke them
prune # also nukes

# *** strategy: remove dups that are known in the copy you want to keep (desired golden copy)
fdupes --delete backup1 backup2 backup3
# I want to get rid of any file in backup2/3 that is in backup1, but not touch other files in backup2/3 (not in backup1)
sel backup1 # select all files in backup1
isel # invert that selection (will select only files in backup1 that are also in backup2/3), does not select files only in backup2/3 b/c it had to first be in backup1
# it's not a global invert, it's per duplicate group
ds # mark for delete (in backup2/3)
prune # done! all files in backup1 are not in backup2/3
# at this point you might be able to manually review the rest if its small enough and you aren't comfortable with fdupes for some reason! that is the benefit of this strategy, if you pick a dir that has most of the files then this alone takes care of most issues
# eventually, I would reocmmend merging the final files into one directory after all dups are removed, unless they are supposed to be separate dirs and just happened to have some dups

#NOTES:
# - pefectly fine to delete subsets and then re-review and target next subset! 
# - duplicate groups drop off as they are removed (once only one file is left), think checklist
# - programatically mark files is fantastic, superior to one by one marking

# if backspace isn't working, just hit ENTER with invalid command and then try again

# to quit:
exit
yes

````

## Cautions

- Be careful with symlinks and hard links. If the same file is included twice, it will nuke one (and thus both) of them!
