---
layout: post
title: "Useful Snippets For macOS"
categories:
  - Tutorials
tags:
  - English
  - software
  - cli

last_modified_at:
excerpt_separator: <!-- more -->
---


<!-- more -->

## Convert an ISO to IMG
```
hdiutil convert -format UDRW -o \~/path/to/target.img \~/path/to/ubuntu.iso
diskutil list
diskutil unmountDisk /dev/diskN
sudo dd if=IMAGE.img of=/dev/diskN bs=1m
diskutil eject /dev/diskN
```

## Copy a file to the clipboard from the Terminal
`cat ~/.bashrc | pbcopy`

## Dock settings
```
defaults write com.apple.dock no-bouncing -bool TRUE

defaults write com.apple.dock autohide-delay -int 0
defaults write com.apple.dock autohide-time-modifier -float 0.4
killall Dock
```

## Remove all DS_Store files globally to reset Finder view settings
`sudo find / -name ".DS\_Store"  -exec rm {} \;`

## SSH

### Generate key
`ssh-keygen -t rsa -b 4096`

### Add key to ssh-agent (-K to permanently add)
`ssh-add -K ~/.ssh/~<NAME OF KEY>~`

### List added identities
`ssh-add -l`

### Use specific port
`ssh -p 3022 user@localhost`

## Pinboard.in: Add Bookmark to specific tag (Bookmarklet)

```
javascript:q=location.href;if(document.getSelection){d=document.getSelection();}else{d='';};p=document.title;void(open('https://pinboard.in/add?later=yes&noui=yes&jump=close&tags=TAGS GO HERE&url='+encodeURIComponent(q)+'&description='+encodeURIComponent(d)+'&title='+encodeURIComponent(p),'Pinboard','toolbar=no,width=100,height=100'));
```