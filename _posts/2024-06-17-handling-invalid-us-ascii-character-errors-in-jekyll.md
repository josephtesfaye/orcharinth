---
title: "Handling Invalid US-ASCII Character Errors in Jekyll"
excerpt:
last_modified_at: 2024/06/17
<!-- author: chatgpt -->
categories:
  - Emacs
tags:
  - Spacemacs
  - Eshell
---

When working with Jekyll, a static site generator commonly used for blogs and documentation, you may encounter errors related to character encoding, particularly the "Invalid US-ASCII character" error. This can happen when Jekyll tries to process files that contain characters not supported by the default US-ASCII encoding. Here’s a detailed look at this error and how to resolve it.

## Understanding the Error

The error message typically looks like this:

```sh
Invalid US-ASCII character "\xE2" on line 54
jekyll 3.9.5 | Error:  Invalid US-ASCII character "\xE2" on line 54
/Users/yourusername/.gem/ruby/3.1.3/gems/jekyll-sass-converter-1.5.2/lib/jekyll/converters/scss.rb:123:in `rescue in convert': Invalid US-ASCII character "\\xE2" on line 54 (Jekyll::Converters::Scss::SyntaxError)
```

This error indicates that Jekyll encountered a character (`"\xE2"`, which is `â` in UTF-8) that is not part of the US-ASCII character set while processing your SCSS files.

## Reproducing the Error

While this error can occur in various environments, let’s consider a scenario where it happens in Emacs's `eshell`, while the same command runs without issue in a standard terminal like zsh.

```sh
~/projects/josephs-blog $ bundle exec jekyll serve -P 4001
```

#### Why This Happens

The default character encoding in `eshell` might be set to US-ASCII, which cannot handle characters beyond the standard ASCII set. This is why characters like `â` (`"\xE2"`) cause problems.

#### Solution

To fix this error, you need to ensure that your environment uses UTF-8 encoding. Here’s how you can do it in both zsh and eshell.

##### 1. Setting Encoding in zsh

Add the following lines to your `~/.zshrc` file:

```sh
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

##### 2. Setting Encoding in Emacs eshell

Add the following to your Emacs configuration file (`~/.spacemacs` or `~/.spacemacs.d/init.el`):

```elisp
(defun dotspacemacs/user-config ()
  ;; other configurations...

  ;; Set UTF-8 as the default encoding for eshell
  (setenv "LANG" "en_US.UTF-8")
  (setenv "LC_ALL" "en_US.UTF-8")
)
```

Reload your Emacs configuration by restarting Emacs or using `SPC f e R`.

#### Ensuring UTF-8 in Your Files

Make sure your files are saved with UTF-8 encoding. In most text editors, you can specify the encoding when saving a file. For Emacs users, you can add the following at the top of your SCSS file:

```scss
// -*- coding: utf-8 -*-
```

This line tells Emacs to interpret the file as UTF-8.

#### Conclusion

Handling character encoding issues can be tricky, but by ensuring that your environment consistently uses UTF-8, you can avoid errors related to unsupported characters. Whether you're using zsh, eshell, or any other terminal emulator, setting the correct encoding is crucial for smooth operation with Jekyll and other tools.

By following the steps outlined above, you should be able to resolve the "Invalid US-ASCII character" error and continue developing your Jekyll site without further issues. Happy coding!
