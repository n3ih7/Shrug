# Shrug

Shrug is a subset of the version control system Git written by Dash.

[Dash (Debian Almquist shell)](https://en.wikipedia.org/wiki/Debian_Almquist_shell) is a modern POSIX-compliant implementation of`/bin/sh`[(sh, Bourne shell)](https://en.wikipedia.org/wiki/Bourne_shell). Dash is not [Bash](https://wiki.archlinux.org/index.php/Bash) compatible, but Bash tries to be mostly compatible with POSIX, and thus Dash.

Here are the functions accomplished:

### shrug-init

The`shrug-init`command creates an empty Shrug repository.

`shrug-init`should create a directory named`.shrug`, which it will use to store the repository. It should produce an error message if this directory already exists.

```
$ ls -d .shrug
ls: cannot access '.shrug': No such file or directory
$ shrug-init
Initialized empty shrug repository in .shrug
$ ls -d .shrug
.shrug
$ shrug-init
shrug-init: error: .shrug already exists
```

### shrug-add filenames...

The`shrug-add`command adds the contents of one or more files to the "index".

Files are added to the repository in a two step process. The first step is adding them to the "index".

### shrug-commit -m message

The`shrug-commit`command saves a copy of all files in the index to the repository.

A message describing the commit must be included as part of the commit command.

### shrug-log

The`shrug-log`command prints a line for every commit made to the repository: each line should contain the commit number, and thecommit message.

### shrug-show [commit]:filename

The`shrug-show`should print the contents of the specified filename as of the specified
commit. If commit is omitted, the contents ofthe file in the index should be printed.

```
$ ./shrug-initInitialized empty shrug repository in .shrug
$ echo line 1 > a$ echo hello world >b$ ./shrug-add a b$ ./shrug-commit -m 'first commit'Committed as commit 0$ echo line 2 >>a$ ./shrug-add a$ ./shrug-commit -m 'second commit'Committed as commit 1$ ./shrug-log1 second commit 
0 first commit$ echo line 3 >>a$ ./shrug-add a$ echo line 4 >>a$ ./shrug-show 0:aline 1 
$ ./shrug-show 1:aline 1
```

### shrug-commit [-a] -m message

`shrug-commit`can have a`-a`option, which causes all files already in the index to have their contents from the current directoryadded to the index before the commit.

### shrug-rm [--force] [--cached] filenames...

`shrug-rm`removes a file from the index, or from the current directory and the index. 

If the`--cached`option is specified, the file is removed only from the index, and not from the current directory.`shrug-rm`, like`git rm`, will stop the user accidentally losing work, and will give an error message instead if the removal would cause the user to lose work.

The`--force`option overrides this, and will carry out the removal even if the user will lose work.


### shrug-status

`shrug-status`shows the status of files in the current directory, the index, and the repository.

```
$ ./shrug-initInitialized empty shrug repository in .shrug
$ touch a b c d e f g h$ ./shrug-add a b c d e f$ ./shrug-commit -m 'first commit'Committed as commit 0$ echo hello >a$ echo hello >b$ echo hello >c$ ./shrug-add a b$ echo world >a$ rm d$ ./shrug-rm e$ ./shrug-add g$ ./shrug-statusa - file changed, different changes staged for commit 
b - file changed, changes staged for commit 
c - file changed, changes not staged for commit 
d - file deleted 
e - deleted 
f - same as repo
```