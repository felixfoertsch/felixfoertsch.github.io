---
layout: post
title: "Use Picture-in-Picture Mode on macOS Mojave with a Bookmarklet"
categories:
  - Tips
tags:
  - english
  - tips
  - macOS

last_modified_at:
excerpt_separator: <!-- more -->
---

[Quinn](https://twitter.com/SnazzyQ) from [Snazzy Labs](https://www.youtube.com/channel/UCO2x-p9gg9TLKneXlibGR7w) just posted an [interesting video](https://www.youtube.com/watch?v=cqjpa8-Cp-s) about some macOS utilities. I love small utilities, but I like it even better, when a problem can be solved with system functions. That is the case here. He mentions Helium - an app to use a form of picture in picture mode for websites that don't support the native PIP.

However, there is a better solution!

<!-- more -->

You can use a small [bookmarklet](https://en.wikipedia.org/wiki/Bookmarklet) that pushes the current HTML5 video on screen into the native PIP. And yes, it also works with Netflix, **if** you are on macOS Mojave (as far as I can tell, Netflix doesn't run in the previous versions of Safari).

Create a new bookmark, enter the following string as the address, give it a nice emoji (like this: ⤵️), voilà! You can now use the native PIP for nearly all your videos!
Here is the code:

```
javascript:document.querySelector(%22video%22).webkitSetPresentationMode(%22picture-in-picture%22);
```


