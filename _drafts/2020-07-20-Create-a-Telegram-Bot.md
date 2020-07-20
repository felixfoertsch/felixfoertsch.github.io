---
layout: post
title: "Create a Telegram Bot"
categories:
  - Code
tags:
  - English
  - software

last_modified_at:
excerpt_separator: <!-- more -->
---

Telegram is a nice messenger. Even though it **absolutely _cannot_ be recommended** if you need security and privacy---I can't stress this enough: Telegram **is not secure**---, it offers very nice, fast and convenient features. One feature I particularly like are stickers. They basically are over sized emoji that give artists a lot of freedom to express emotion. These stickers are collected in packs that can be added to your client.

They have one problem though: Many times a sticker pack has only a few really good stickers.

To get around this problem Telegram offers a system to put stickers on a list of favorites. This is, in theory, a very good idea, since this allows to collect your favorite stickers from all the packs that are floating around. However, the clients implemented a very confusing design decision (or is it a bug?). The list of favorites is only 5 stickers wide; and the moment you add another sticker, the oldest one gets pushed into nothingness.

And because I am a sticker addict this problem is getting worse and worse for me. There are two solutions to this problem: 1) Contribute to the source code of Telegram and try to fix this bug, 2) Create a Telegram bot, that can curate a personal sticker collection.

Since I participated in the GitHub repository of Telegram in the past---it wasn't a good experience---I decided to try myself on a Telegram Bot. This way, I am not reliant on anybody.

<!-- more -->

## The Goal

The way the bot should behave is pretty simple. I want to be able to send the bot a sticker and it should put it in the curated pack. It should then update the pack so all the clients receive the updated sticker pack. Ultimately I want to only subscribe to my personal sticker pack that contains all my favorites.

## Reading the Docs

Using [@stickerdownloadbot](https://telegram.me/stickerdownloadbot) to download the pictures.

Using the official [@stickers](https://telegram.me/stickers) to create and manager the sticker pack.
https://telegramguides.com/create-telegram-sticker-packs/


