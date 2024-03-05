---
title: Common Screen Commands
date: 2024-03-05 16:37:26
toc: true
tags:
    - Project Tools
categories: blog
excerpt: This is a brief introduction about the common commands of the Screen utility on the Linux terminal.
---
## Introduction
In project development, when executing programs on the Linux terminal, if the terminal is closed, the program execution will also terminate. This poses significant inconvenience for long-running programs.

Screen facilitates the management of multiple command-line workflows without concern for their interference. Programs are automatically backgrounded and continue execution until completion.
## Start a New `screen` Session
```bash
# Start a new screen session named "my_session"
screen -S my_session
# Automatically name the new screen session
screen
```
## View Existing `screen` Sessions
```bash
# List all screen sessions
screen -ls
```
## Attach to an Existing `screen` Session
```bash
# Attach to the screen session named "my_session"
screen -r my_session
```
## Detach from an Existing `screen` Session
```bash
# Way 1
# Detach from the screen session named "my_session"
screen -d my_session
# Way 2
# Enter the following keys in turn and the program
# will also continue to execute in the background
Ctrl+a
a
```
## Delete an Existing `screen` Session
```bash
# Delete the screen session named "my_session"
screen -X -S my_session quit
```
