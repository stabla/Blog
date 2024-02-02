---
layout: post
title: "Golang install issue fixing: Package is not in GOROOT"
description: "How to fix the error package is not in GOROOT?"
date: 2024-02-02
tags: [article, issue, fix, golang]
comments: false
share: true
---
# Resolving $GOROOT Issue: Save Time with Quick Fixes

Time is precious, so let's get straight to the point.

## The Problem
Recently, while attempting to install a package using Go, I encountered an unusual error:

```bash
package crypto/ecdh is not in GOROOT
```

Before running these commands, I would invite you to understand them and not trust blindly an internet article. However, if you're looking for the solution and knows about the commands listed below, just continue reading.

To swiftly resolve this issue, follow these simple steps:

Remove Existing Go binaries
```bash
sudo rm -rf /bin/go
```


Update the package list and address any missing dependencies:
```bash
sudo apt update --fix-missing
```

Reinstall Golang with all the binaries
```bash
sudo apt install golang -y
```

By executing these commands, you should be able to overcome the $GOROOT issue and continue with your Go package installation seamlessly. 

Remember, efficiency matters – let's make the most of our time!
