---
title: "01 Pre-class setup"
format:
  html:
    code-fold: true
    toc: true
    toc-location: left
editor: visual
---

In this chapter you will learn how to setup your environment either on the remote server or locally.

**Learning objectives**

1. Create directory
2. Copy repo
3. Setup conda environment
4. Download data

# Setup directory

## Login to AUTH cluster (Remote coding only)

Before setting up, there are some necessary steps specific for remote coding which are specific to Windows users. If you are a Linux os MacOS user skip to [section](#unix-linuxmacos-users)

### Install VSCode or MobaXTerm (Windows Users Only)

In order to use SSH (remote host access) to the AUTH computer cluster you will either need to have Windows Subprocesses for Linux [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install) installed and enabled or use and IDE such as [VSCode](https://code.visualstudio.com/) or [MobaXTerm](https://mobaxterm.mobatek.net/)

### Connect to AUTH cluster

If you are not logged in an AUTH network (e.g. working from home), make sure you have eduVPN enabled. More info [here](https://it.auth.gr/manuals/eduvpn/)

Then open a terminal window or your IDE and type the following:

```{bash}
ssh [username]@aristotle.it.auth.gr
```

## Download repo

```{bash}
git clone https://github.com/ggeorgol/ATACseq_course

cd ATACseq_course
```