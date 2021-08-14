---
layout: post
comments: true
title: Tutorial 6. Navigating the Shell
---

### Learning objectives

- Understand what the shell is and become familiar with basic commands used to create, access, and remove files and directories
- Use additional commands to manipulate files and their relative locations

## Introduction

The shell is a program that takes commands from the keyboard and gives them to the operating system to perform. In the old days, it was the only user interface available on a Unix-like system such as Linux. Nowadays, we have *graphical user interfaces (GUIs)* in addition to *command line interfaces (CLIs)* such as the shell.

On most Linux systems a program called **[bash](http://www.linuxcommand.org/lc3_man_pages/bash1.html)** (which stands for Bourne Again SHell, an enhanced version of the original Unix shell program, **sh**, written by Steve Bourne) acts as the shell program. Besides **bash**, there are other shell programs available for Linux systems. These include: **ksh**, **tcsh** and **zsh**.

## Important Commands

***pwd***

pwd stands for “present working directory”. Implementing this command will return the current full directory.

```bash
$ pwd
/User/home/
```

***ls***

ls stands for “list”. Implementing this command will return the full list of files and folders under current directory.

```bash
$ ls
folderA folderB folderC documentD
```

***mkdir***

Mkdir or “make directory” is a command used to create a folder of your choice. For example, to create the directory “EXAMPLE” in your current working directory, execute the command mkdir EXAMPLE.

```bash
$ mkdir EXAMPLE
$ ls
EXAMPLE
```

***cd***

cd stands for “change directory”. Usually, we can implement cd command by `cd NEW_DIRECTORY`. “NEW_DIRECTORY” can be a full directory path, or other notations. For example, simply replacing "NEW_DIRECTORY" with the name of the folder under your current path can change your current directory to the target folder. Typing “../” can make directory go backward to the previous directory. Typing “~” can make directory go to the default home directory. Typing “/” can make directory go to the root directory. Also, typing “-” can make directory go to the previous directory.

```bash
$ pwd
/User/home/
$ cd /User/home/folderA
$ pwd
/User/home/folderA
$ cd folderZ
$ pwd
/User/home/folderA/folderZ
$ cd ../
$ pwd
/User/home/folderA
$ cd ~
$ pwd
/User/home/ 
$ cd /
$ pwd
/
$ cd -
$ pwd
/User/home/
```

***rm***

rm or “remove” is a command that can be used to both remove files and directories. For example, to remove a file in your current working directory, execute `rm filename`. To remove a directory and all files within said directory, use `rm -r directoryname` to recursively remove every file inside and the directory itself.

***cp***

cp or “copy” is a command that can be used to copy files or folders from one location to another. For example, to copy a single file from your current working directory to the target directory, execute `cp filename targetdirectory`. To copy a directory from one location to another, use `cp -r sourcedirectory targetdirectory`, to recursively copy the directory and all files within it.

***mv***

mv or “move” is a command that can move files or folders from one location to another and rename files. Like cp, to move files or folders, execute `mv filenameOrDirectory targetdirectory`. Remember to use the recursive flag if moving the entire folder. To rename a file or directory, execute `mv filename newfilename` or `mv directoryname newdirectoryname`.

***grep***

grep or “global regular expression print” is a command that searches files for strings. To search a string, execute `grep searchstring filename`. This command will return *filename: line with searchstring.* To search for a string with more than one word, just add quotes around the string: “*I am looking for this* *search string”*  **There are many useful options for the grep command that will change its output:

-r  recursive, will search for the string in subdirectories

-l list names of files with the string

-i ignore case

-c count number of matches

***?***

The question mark is a single character wild card in bash. This means that any time it appears in conjunction with the above commands (e.g. in a filename or directory name), it can be replaced with any one character.

 *****

The asterisk is another wild card in bash. It can stand for any number of characters, including no characters.

***vim***

vim or “visual improved” is a Unix text editor.

## Try and Learn

Launch the terminal. In the command line try to execute the following tasks with what you know:

1. Create 2 directories: “folder” and “folder1”
2. Enter “folder”
3. Create and save a txt file called “doc” with the contents “Hello world!”
4. Copy this file to “folder1”
5. Return back to your home directory and remove “folder”
6. Rename “folder1” to “example”
7. Within “example”, search for every instance of “world” in “doc”

Congratulations! You have mastered navigating the shell.
