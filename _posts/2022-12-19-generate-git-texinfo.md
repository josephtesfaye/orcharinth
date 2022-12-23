---
title: "Generate and use TexInfo documentation for Git"
excerpt: "How to produce texinfo files for Git"
last_modified_at: 2022-12-19
categories:
  - Emacs
tags:
  - Git
---

**Info** is a documentation reader for reading Info files, which ususally have the extension `.info`. Info files are generally produced from **Texinfo** files, which have the extension `.texi`, and with the `makeinfo` command. See [GNU's website](https://www.gnu.org/software/texinfo/) for more details about Texinfo.

Emacs has its own built-in Info reader (start with `C-h i`). So how do you install an Info documentation for Emacs? 

There are several ways to view additional Info manuals in Emacs as described in [Emacs FAQ](https://www.gnu.org/software/emacs/manual/html_node/efaq/Installing-Texinfo-documentation.html). Here is what I think is the best set-up - using an addtional info directory:

First create a directory that will contain the external Info files, such as `~/.local/share/info`. Then copy the `.info` files to be installed to this directory. A special file named `dir` listing the nodes to display in the Info reader needs to be created in this directory as well. If the `install-info` command (which is packaged with the program `texinfo`) is available on your system, you can add the files to `dir` by invoking `install-info <manual>.info dir`. If it\'s not installed you can edit `dir` directly. First copy the `dir` file under the installation directory of Emacs (such as `c:/Program Files/Emacs/x86_64/share/info/dir`) to the external directory. Then follow the examples already in that file to add entries for the `.info` files you\'re installing, which have the format:

``` example
* Topic: (relative-pathname).  Short description of topic.
```

where `relative-pathname` corresponds to the relative path of the file without the `.info` suffix. The `dir` file should list only entries for Info files in that directory, and all other contents could be cleared except these lines before the entries:

``` example

File: dir,  Node: Top   This is the top of the INFO tree

* Menu:

```

Now add the external directory to the `Info-additional-directory-list` to make Emacs aware of it:

``` example
(add-to-list 'Info-additional-directory-list "~/.local/share/info")
```

Finally kill all `*info*` buffers and open a new `*info*` buffer again (no need to restart Emacs). You should see the manuals in the top node.

### References

-   Info: An introduction ([Info](info:info))
-   GNU Texinfo Manual ([Info](info:texinfo), [website](https://www.gnu.org/software/texinfo/manual/texinfo/))
-   [Building TexInfo version of Git User Manual and Git Manual Pages](git.org::#build-texinfo)
