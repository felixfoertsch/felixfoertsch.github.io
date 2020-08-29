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

I like [Telegram](https://telegram.org). Even though it **absolutely _cannot_ be recommended** if you need security and privacy---I can't stress this enough: Telegram **is not secure**---, it offers very nice, fast and convenient features. One feature I particularly like are stickers. They basically are over sized emoji that give artists a lot of freedom to express emotion. These stickers are collected in sets that can be added to your client. Look at this cool Dino:

<video controls>
 <source src="/img/20200827-dino.mp4" type="video/mp4">
</video>

They have one problem: Many times a sticker set has only a few really good stickers.

To get around this problem Telegram offers a system to collect favorite stickers. This is a very good idea. But only in theory. The clients implement a very confusing design decision (or is it a bug?). The list of favorites is only 5 stickers wide; and the moment you add another sticker, the oldest one gets pushed into nothingness. 5 favorites are not enough.

There are two solutions to this problem: 1) Contribute to the source code of Telegram and try to fix this bug, 2) Create a Telegram bot, that can curate a personal sticker collection.

Since I participated in the GitHub repository of Telegram in the past and it wasn't a good experience, I decided to try myself on a Telegram Bot. This way, I am not reliant on anybody and I can learn something in the meanwhile.

<!-- more -->

## The Goal

Creating a bot. Send a sticker to the bot and it puts it in a curated set; curated by the user. Ultimately this makes it possible to only be subscribed to a personal sticker set that contains all favorite stickers of the user.

## Preparation

### First Decisions

The first step for any project is reading available documentation to better understand the problem space. Luckily, the [Telegram Bots API](https://core.telegram.org/bots/api) is very well documented. It also contains a [comprehensive list of Bot API implementations](https://core.telegram.org/bots/samples).

I decided to use the programming language Java for the bot implementation, since I am most comfortable with it. I will use [rubenlagus's TelegramBots](https://github.com/rubenlagus/TelegramBots) API implementation. The *test* bot will be hosted on my Raspberry Pi, the *production* bot on my [Uberspace](https://uberspace.de).

### Telegram API Overview

To identify and understand how to use the API, my first step is looking through the [available types](https://core.telegram.org/bots/api#available-types). The types and attributes of interest are:

- User:
    - id (necessary to attribute the sticker set)
    - username (to construct the sticker set name)
- Chat:
    - id (to respond to the correct chat, post status messages, etc.)
- Message:
    - message_id
    - from -> Returns the User (see above)
    - chat -> Returns the Chat (see above)
    - reply_to_message (reply to the posted sticker), text (a field where we can receive text and communicate with the user)
    - sticker -> Returns a/the Sticker (see below)
- BotCommand:
    - command
    - description
- Sticker:
    - file_id
    - is_animated -> Important, since regular and animated stickers can't be in the same set

It seems to be possible to extract the required information from the Message object. However, there are limits to the sticker sets: 50 in animated sticker sets and 120 in unanimated sets. Once we reach the limit, we need to create a new set. This means we need to somehow keep state or have the user keep track of it.

### Understanding the Connection to the Java Implementation

After adding all required dependencies, the entry point of the Java API looks like this:

```java
public class Main {

 public static void main(String[] args) {
 ApiContextInitializer.init();
 TelegramBotsApi telegramBotsApi = new TelegramBotsApi();
 try {
  telegramBotsApi.registerBot(new FavoriteStickersBot());
 } catch (TelegramApiException e) {
  e.printStackTrace();
 }
 }
}
```

We initialize the API implementation and then we register our custom `FavoriteStickersBot` class with it. There are different ways to implement a bot, but we will go with the *Getting Started* way and extend `TelegramLongPollingBot`. [Long polling](https://en.wikipedia.org/wiki/Push_technology#Long_polling) is a method that enables the system to use push behavior without actually having push capabilities.

```java
 public class FavoriteStickersBot extends TelegramLongPollingBot {
 public String getBotUsername() {
 return BotConfig.BOT_USERNAME;
 }

 public String getBotToken() {
 return BotConfig.BOT_TOKEN;
 }

 public void onUpdateReceived(Update update) {}
```

When running the program, the bot will execute and

The Java implementation basically uses a 1:1 mapping of the API to Java. To instantiate a Telegram User object you can either use `User user = new User()` and then use the setters

Methods are implemented the similarly. To create a new sticker set we do the following:

```java
CreateNewStickerSet createNewStickerSet = new CreateNewStickerSet()`. We then
```

## Implementation

### Project setup

Following the [Getting Started Guide](https://github.com/rubenlagus/TelegramBots/wiki/Getting-Started) from TelegramBots, the first step is registering your new and shiny bot with the Telegram [@BotFather](https://telegram.me/BotFather). During this process you will receive the required API token to enable communication between the bot and the Telegram service.

## Potential Pitfalls

I am using the file_id instead of creating the stickers from scratch

## Next Steps

Currently, the bot is single-user only. It should be reasonably easy to add multi-user support by creating a group, adding the bot to it and then having a shared best sticker set.
