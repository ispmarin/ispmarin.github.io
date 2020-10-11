---
title: Encrypted Backup Redux
categories: linux
date: 2020-04-12
tags:
  - linux
  - debian
  - system
  - backup
  - encryption
---

gpgzip seems to be deprecated, and the recommended tool to replace it is called [gpgtar](https://www.gnupg.org/documentation/manuals/gnupg/gpgtar.html). It's easy to use it, but the documentation doesn't make it clear that it can compress the file, the option of symmetric key didn't work, as I don't want to mess with encryption keys when working with backups, as I think that the possibility of losing everything, _including_ the encryption keys, is too big, so I want to use passwords. I decided to use the good old Linux pipe, with a simple command:

```
tar czvpf - folder_to_backup | gpg --symmetric --cipher-algo aes256 -o backup_filder.tar.gz.gpg
```

and that's it. I know how it was compressed and how it was encrypted. I can copy it again using rclone, as I [previously explained](https://ispmarin.github.io/linux/backup-with-gdrive-rclone-gpg/). Simple and it works, even though it is not as automated as I would like. 