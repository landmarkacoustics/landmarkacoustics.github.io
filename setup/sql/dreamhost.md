---
title: Dreamhost SQL Servers for Django Sites
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Dreamhost SQL Servers for Django Sites

[Dreamhost][4] lets you set up virtual servers that run MySQL.
You don't have to do any system maintenance for them, but you don't have total
control over the server's settings. In particular, Django complains because the
default Dreamhost setting is to not use "strict" MySQL. I don't know, yet,
whether that is a problem, but Dreamhost has [a more expensive option][1] that
takes care of it if you need to use it. You have to [email customer support][2]
to do it, though.

If you don't need to stress about strictness then setting up a MySQL server is
[ridiculously easy][3].

[1]:https://help.dreamhost.com/hc/en-us/articles/215945927-How-do-I-add-or-delete-a-MySQL-VPS-
[2]:https://help.dreamhost.com/hc/en-us/articles/215722447-Contacting-DreamHost-via-email
[3]:https://help.dreamhost.com/hc/en-us/articles/221691727-Creating-a-MySQL-database
[4]:https://www.dreamhost.com
