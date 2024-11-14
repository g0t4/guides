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

# to quit:
exit
yes

````

## Cautions

- Be careful with symlinks and hard links. If the same file is included twice, it will nuke one (and thus both) of them!
