---
categories:
- DFIR
- macOS
date: "2022-06-16T00:00:00Z"
tags:
- sqlite
- plist
title: macOS Application Forensics
---

## Background and Setup
When investigating installed applications on macOS, the artifacts will likely be contained within either a [Property List](https://en.wikipedia.org/wiki/Property_list)  (`.plist`) file or an [SQLite](https://www.sqlite.org/index.html) file. Both of these files are common across Apple's ecosystem, including it's mobile and IoT operating systems. 

The best platform to examine macOS applications is from a macOS system. Installing [Xcode](https://developer.apple.com/xcode/resources/) developer tools for macOS will give the ability to read both types of plist files, and a FOSS tool like [DB Browser for SQLite](https://sqlitebrowser.org/dl/) will allow opening and exploring SQLite files and doing queries on data in different tables.

## Where To Look
Tools are great, but you have to know where to point them to get the answers you need. By default, macOS uses [System Integrity Protection](https://support.apple.com/en-us/HT204899) to limit **3rd party** installers to the following directories:

- `/Applications`
-  `/Library`
-  `/usr/local`

This means that if you are examining an **Apple Native** application such as Safari, there might be artifacts outside these locations. These files are hidden by default when viewing with Unix-type OSes. With macOS, `CMD + SHIFT + .` while in the Finder window will show hidden system files.

### The Home Directory
The user's home directory (~) usually contains the bulk of forensically-relevant files for specific applications. Some programs use "dotfiles" (files or directories prepended with `.` which are hidden) which are at the root of the user's directory, and could contain relevant information.

![Hidden folder for Visual Studio Code](Pasted%20image%2020220613212422.png)

#### ~/Library
The user's Library directory is where most application-specific files are found. There are several supporting directories.

First, the application will have a discrete place in the user's Library directory.

![](Pasted%20image%2020220613222840.png)

The above screenshot is the directory for the Reminders application. There are several SQLite database files in sub-directories as well as a plist file. Double-clicking on the .plist file with Xcode tools installed will show the contents whether it is a regular or binary format. 

![](Pasted%20image%2020220613223534.png)

> In this case, the `AccountInformation.plist` file contains no relevant information.

However the Files directory contains attachments embedded within individual entries.

![](Pasted%20image%2020220613223734.png)

And the Stores directory contains SQLite files.

![](Pasted%20image%2020220613221659.png)

Opening the `Data-local.sqlite` file with DB Browser for SQLite will let us see that data. 

![](Screen%20Shot%202022-06-16%20at%2012.03.32.png)

Clicking on the 'Brows Data' button will then show a list of tables that can be selected to view the contents.

![](Screen%20Shot%202022-06-16%20at%2012.03.03.png)

Each table will contain either a standard database with schema, or a binary blob which can be exported and viewed externally. The contents of these SQLite and plist files will vary depending on the application. We'll explore these in depth in another article.

## Application Sandboxing
The other thing to be aware of is that macOS applications are run in a sandbox. Sandboxed applications are loaded only with permissions granted to it, and a virtual environment is created for the application which mimics the directory structure on disk via *aliased directories* which have permissions set for read and/or write.

Two main folders used are in the User's Library folder:
- `~/Library/Containers`
- `~/Library/Group Containers`

![](Pasted%20image%2020220616122159.png)

These aliased folders will give access to other parts of the file system when the application requests it and the user grants permission. 