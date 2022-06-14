---
title: macOS Application Forensics
date: 2022-06-13 15:04 -0400
categories: [Forensics, macOS]
tags: [sqlite, plist, ]  # TAG names should always be lowercase
---

## Background and Setup
When investigating installed applications on macOS, the artifacts will likely be contained within either a [`.plist`](https://en.wikipedia.org/wiki/Property_list)  (which can be a regular or binary format) or an [`SQLite`](https://www.sqlite.org/index.html) file. Both of these files are common across Apple's ecosystem, found in both iOS and Android mobile operating systems as well. 

The best platform to examine macOS applications is from a macOS system. Installing [Xcode](https://developer.apple.com/xcode/resources/) developer tools for macOS will give the ability to read both types of plist files, and a tool like [DB Browser for SQLite](https://sqlitebrowser.org/dl/) will allow opening and exploring SQLite files and doing queries on data in different tables.

## Where To Look
Tools are great, but you have to know where to point them to get the answers you need. By default, macOS uses [System Integrity Protection](https://support.apple.com/en-us/HT204899) to limit **3rd party** installers to the following directories:

- `/Applications`
-  `/Library`
-  `/usr/local`

This means that if you are examining an **Apple Native** application such as Safari, there might be artifacts outside these locations. These files are hidden by default when viewing with Unix-type OSes. With macOS, `CMD + SHIFT + .` will show hidden system files.

### The Home Directory
The user's home directory (~) usually contains the bulk of forensically-relevant files for specific applications. Some programs use "dotfiles" (files or directories prepended with `.` which are hidden) which are at the root of the user's directory, and could contain relevant information.

![Hidden folder for Visual Studio Code](/assets/images/Pasted%20image%2020220613212422.png)

#### ~/Library
The user's Library directory is where most application-specific files are found. There are several supporting directories.

First, the application will have a discrete place in the user's Library directory.

![](/assets/images/Pasted%20image%2020220613222840.png)

This is the directory for the Reminders application. The `AccountInformation.plist` file contains no relevant information.

![](/assets/images/Pasted%20image%2020220613223534.png)

However the Files directory contains attachments embedded within individual entries.

![](/assets/images/Pasted%20image%2020220613223734.png)

![](/assets/images/Pasted%20image%2020220613221659.png)

The above screenshot is the directory for the Reminders application. There are several SQLite database files as well as a `.plist` file.