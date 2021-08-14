---
layout: post
comments: true
title: Tutorial 9. Text Editing in the Linux environment
---

### Learning objectives

- Use vim to create, edit, and manipulate text files
- Use additional commands to extract data from text files

## Introduction

Vim (visual improved) is a text editing program that works directly in the terminal. Its functionality is based on working in modes. By switching between modes, you can write, edit, navigate, and save your work without taking your hands off the keyboard.

## Essential modes

Vim is a powerful program with many options, but you can get started with a just a few modes. To create a new text file, execute `vim file.txt`. This opens a new file called file.txt, which you can edit using the following modes:

| Mode | Description |
| --- | ----------- |
| i | insert, allows you to edit the text |
| :wq | write quit, saves changes and exits the file |
| q! | quit, exits the file without saving changes |

When you are ready to exit the insert mode, type `Esc` and vim will return to the normal mode that it opened in. From here, you can type the letters for other modes and then type `Enter` to execute them.

## Other useful modes

| Mode | Description |
| --- | ----------- |
| a | insert after the cursor |
| A | insert at the end of a line |
| v | visual, selects text one *character* at a time|
| V | visual, selects text one *line* at a time|
| v + Ctrl | visual, selects text as a block|
| y | yank, copy selected text |
| d | delete selected text |
| p | paste text after cursor |
| gg | moves to the top of the file |
| G | moves to the bottom of the file |

## Advanced examples

##### Searching for a string
`/string` will search your text file from *inside vim* for the string. Type `n` to move fowards in your search and `N` to move backwards.

##### Find and replace
`:%s/old_text/new_text/g` will search for all instances of the string old_text and replace it with the string new_text .

##### Insert a word at the beginning of each line
`Ctrl + V`, `G`, `Ctrl + i`, `new_word`, `Esc`  
These commands will enter visual block mode, select the first character of each line, enter insert mode at the cursor (the entire selected section), and insert the new text.

## Try and Learn

Launch the terminal. In the command line try to execute the following tasks with what you know:

1. Create 2 directories: “folder” and “folder1”
2. Enter “folder”
3. Create and save a txt file called “doc” with the contents “Hello world!”
4. Copy this file to “folder1”
5. Return back to your home directory and remove “folder”
6. Rename “folder1” to “example”
7. Within “example”, search for every instance of “world” in “doc”

Congratulations! You have mastered text editing with Vim.
