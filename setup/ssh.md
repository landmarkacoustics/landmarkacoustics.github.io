---
title: Secure Socket Layer
author: Ben Taft
copyright: (C) 2021 Landmark Acoustics LLC
---

# Secure Socket Layer

It seems like, especially if I want to use EMACS to edit files remotely, that I
need to populate a configuration file that links keys on my machine to the
remote hosts those keys provide access to. The file is `~/.ssh/config`. If I
want to connect to `ssh.example.com` using the key in `~/here/is/a/file.pem`
then this is what the lines in the config file should look like:

```{conf}
Host ssh.example.com
    IdentityFile ~/here/is/a/file.pem
```

