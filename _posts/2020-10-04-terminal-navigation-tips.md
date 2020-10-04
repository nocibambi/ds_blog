---
toc: false
layout: post
description: Quick tips for quick navigation in the terminal
categories: [terminal, navigation, tips]
title: Command line tips for quicker navigation interminal
---
Things I wish I had knew earlier.

Most of the tips are [from here](https://superuser.com/questions/356199/best-way-to-gain-quick-access-to-frequently-used-directory-in-linux-terminal)

# Basics

Navigation to any folder

```shell
> cd /home/andras/Projects/data_science_blog/
/home/andras/Projects/data_science_blog
```

With the `~` we can jump to our home folder

```shell
> cd ~
/home/andras
```

# `cd`

We do not even need the `~` to navigate to our home folder.

```shell
> cd /
/
> cd
/home/andras
```

# Bash function

We can also create a bash function so we can spare even typing `cd`

Here is the function

```bash
# .bashrc

function blog() {
    cd "$HOME/Projects/data_science_blog"
}
```

And jump!

```shell
> blog
/home/andras/Projects/data_science_blog
```

# Symbolic link

An another possible way is to create a symbolic link in our home folder.

```shell
> ln -s ~/Projects/data_science_blog/ dsblog
> ls -l ~/dsblog
lrwxrwxrwx 1 andras andras 40 okt    4 10:39 /home/andras/dsblog -> /home/andras/Projects/data_science_blog/
```

And jump!

```
> cd ~/dsblog
/home/andras/dsblog
```

However, because of the way symbolic links work, our 'parent' folder is still the home directory.

```shell
> cd ..
/home/andras
```

# `cd -`

With `-` we can jump back to our previous folder

```shell
> cd -
/home/andras/Projects/data_science_blog
```

# Path variable in `.bashrc`

A further option is to create a variable in bash

```bash
# .bashrc
BLOG=/home/andras/Projects/data_science_blog
```

```shell
cd $BLOG
/home/andras/Projects/data_science_blog
```

# Create an alias

```bash
# .bashrc
alias ds_blog="cd /home/andras/Projects/data_science_blog"
```

```shell
> ds_blog
/home/andras/Projects/data_science_blog
```
