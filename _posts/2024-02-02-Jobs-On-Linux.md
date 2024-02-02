---
layout: post
title: "Linux processes cheatsheet - run them in the background"
description: "How to make run your processes in the background and manager them"
date: 2024-02-02
tags: [article, linux, jobs, processes, background, tasks]
comments: false
share: true
---

# Linux processes: push them in the background

Run your command in the background, you will get the job number
```bash
username@host:~$ python3 mysuperscript.py >> results.txt &
[1] 1940670
```

[1] is the job number
1940670 is the process id

## List of commands you should know

* jobs - list the current jobs
* fg - resume the job that's next in the queue
* fg %[number] - resume job [number]
* bg - Push the next job in the queue into the background
* bg %[number] - Push the job [number] into the background
* kill %[number] - Kill the job numbered [number]
* kill -[signal] %[number] - Send the signal [signal] to job number [number]
* disown %[number] - disown the process(no more terminal will be owner), so command will be alive even after closing the terminal.
* CRTL+Z while a process is running will stop it and put it in the queue, which is listable through the command jobs

Time is precious, save it! 

All the documentation regarding these commands can be found online or on the man pages of Linux.
