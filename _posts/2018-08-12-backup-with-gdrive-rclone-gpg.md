---
title: Encrypted backup with gpg-zip, Google Drive and rclone
categories: linux
tags:
    - backup
    - cloud
    - gdrive
    - rclone
    - gpg
date: 2018-08-12
---

I was using SpiderOak as a backup solution for one year, but it seems that it has some 
[problems](https://www.reddit.com/r/privacy/comments/94nspi/spideroak_cans_its_warrant_canary_suffers/). I'm not
100% sure that this is a real problem, but with its current prices and my access to a Google drive account with a very large
limit, I decided to cancel my account and use GDrive as my backup. But there are a couple of problems:
first, there is no sync client for Linux, and second, I don't want to send my files to Google unencrypted. So I combined
two tools: `gpg-zip`and `rclone`.

GPG-Zip encrypts files or folders into an archive. This makes the job of encrypting a folder much easier: it will create
a tar file and encrypt this tar archive. This solves problem one.

Now, I want to automate this as much as possible. To do this, the `rclone` tool would fit nicely. rclone can upload files
to several cloud providers, including GDrive. 

## Creating the backup
We have to configure rclone before first use. To do this, use `rclone config`. I selected option 7 for Google Drive, and after
naming my backup endpoint and answering a few questions with default options, it opened a browser window to log in into my 
account and created a token. Done! rclone is configured.

Next step is to encrypt my files. I chose one directory and used the command

```bash
gpg-zip -c -o backup_20180812.gpg.zip folder_to_backup
```

and let it run. After it was created, I uploaded to my drive with

```bash
rclone copy backup_20180812.gpg.zip gdrive_remote:backup
```

and waited until it was done. I took 4 hours to upload a 20 GB file on my internet connection, but the file is there. The 
next step is to create a tool that can do it automatically.
