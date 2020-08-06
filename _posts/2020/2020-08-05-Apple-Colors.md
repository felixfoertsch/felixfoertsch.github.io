---
layout: post
title: "Apple Colors in SCSS"
categories:
  - Design
tags:
  - English

last_modified_at: 
excerpt_separator: <!-- more -->
---

Colors have a big impact on how software looks. Personally, I think macOS and iOS are beautiful and the colors are very well selected. Since I am not a designer, but I want to understand **why** these systems always looks so good, I started designing this website using the Apple colors. The first step I took was creating variables for the Apple colors from the [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/) ([iOS](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/)/[macOS](https://developer.apple.com/design/human-interface-guidelines/macos/visual-design/color/)).

I published them on GitHub as gists so you can use them, too. It's just the raw variables; but I could not find them anywhere else in this SCSS form. Gist [macOS-colors-dark.scss](https://gist.github.com/felixfoertsch/52904ab569521158dd8494024c0a5d77), [macOS-colors-light.scss](https://gist.github.com/felixfoertsch/36922ee9a74d9bd0c0887f62cecb7041), [iOS-colors-dark.scss](https://gist.github.com/felixfoertsch/69dd59c9e541788056ee6c70928c3769), [iOS-colors-light.scss](https://gist.github.com/felixfoertsch/e4c73a673bd769218c39c17ac09127aa).

Read more for the actual variables.

<!-- more -->

```scss
// macOS-colors-dark.scss
$macOS-systemBlue-dark: rgba(10, 132, 255, 1);
$macOS-systemBrown-dark: rgba(172, 142, 104, 1);
$macOS-systemGray-dark: rgba(152, 152, 157, 1);
$macOS-systemGreen-dark: rgba(50, 215, 75, 1);
$macOS-systemIndigo-dark: rgba(94, 92, 230, 1);
$macOS-systemOrange-dark: rgba(255, 159, 10, 1);
$macOS-systemPink-dark: rgba(255, 55, 95, 1);
$macOS-systemPurple-dark: rgba(191, 90, 242, 1);
$macOS-systemRed-dark: rgba(255, 69, 58, 1);
$macOS-systemTeal-dark: rgba(100, 210, 255, 1);
$macOS-systemYellow-dark: rgba(255, 214, 10, 1);
$macOS-alternateSelectedControl-dark: rgba(0, 88, 208, 1);
$macOS-alternateSelectedControlText-dark: rgba(255, 255, 255, 1);
$macOS-controlBackground-dark: rgba(30, 30, 30, 1);
$macOS-controlText-dark: rgba(255, 255, 255, 0.8);
$macOS-disabledControlText-dark: rgba(255, 255, 255, 0.2);
$macOS-grid-dark: rgba(255, 255, 255, 0.1);
$macOS-headerText-dark: rgba(255, 255, 255, 1);
$macOS-highlight-dark: rgba(180, 180, 180, 1);
$macOS-labelColor-dark: rgba(255, 255, 255, 0.8);
$macOS-linkColor-dark: rgba(65, 156, 255, 1);
$macOS-placeholderTextColor-dark: rgba(255, 255, 255, 0.2);
$macOS-quaternaryLabelColor-dark: rgba(255, 255, 255, 0.1);
$macOS-secondaryLabelColor-dark: rgba(255, 255, 255, 0.5);
$macOS-selectedContentBackgroundColor-dark: rgba(0, 88, 208, 1);
$macOS-selectedControlColor-dark: rgba(63, 99, 109, 1);
$macOS-selectedControlTextColor-dark: rgba(255, 255, 255, 0.8);
$macOS-selectedMenuItemTextColor-dark: rgba(255, 255, 255, 1);
$macOS-selectedTextBackgroundColor-dark: rgba(63, 99, 139, 1);
$macOS-selectedTextColor-dark: rgba(255, 255, 255, 1);
$macOS-separatorColor-dark: rgba(255, 255, 255, 0.1);
$macOS-shadowColor-dark: rgba(0, 0, 0, 0);
$macOS-tertiaryLabelColor-dark: rgba(255, 255, 255, 0.2);
$macOS-textBackgroundColor-dark: rgba(30, 30, 30, 1);
$macOS-textColor-dark: rgba(255, 255, 255, 1);
$macOS-underPageBackgroundColor-dark: rgba(40, 40, 40, 1);
$macOS-unemphasizedSelectedContentBackgroundColor-dark: rgba(70, 70, 70, 1);
$macOS-unemphasizedSelectedTextBackgroundColor-dark: rgba(70, 70, 70, 1);
$macOS-unemphasizedSelectedTextColor-dark: rgba(255, 255, 255, 1);
$macOS-windowBackgroundColor-dark: rgba(50, 50, 50, 1);
$macOS-windowFrameTextColor-dark: rgba(255, 255, 255, 0.8);
```

```scss
// macOS-colors-light.scss
$macOS-systemBlue: rgba(0, 122, 255, 1);
$macOS-systemBrown: rgba(162, 132, 94, 1);
$macOS-systemGray: rgba(142, 142, 147, 1);
$macOS-systemGreen: rgba(40, 205, 65, 1);
$macOS-systemIndigo: rgba(88, 86, 214, 1);
$macOS-systemOrange: rgba(255, 149, 0, 1);
$macOS-systemPink: rgba(255, 45, 85, 1);
$macOS-systemPurple: rgba(175, 82, 222, 1);
$macOS-systemRed: rgba(255, 59, 48, 1);
$macOS-systemTeal: rgba(90, 200, 250, 1);
$macOS-systemYellow: rgba(255, 204, 0, 1);
$macOS-alternateSelectedControl: rgba(0, 99, 255, 1);
$macOS-alternateSelectedControlText: rgba(255, 255, 255, 1);
$macOS-controlBackground: rgba(255, 255, 255, 1);
$macOS-controlText: rgba(0, 0, 0, 0.8);
$macOS-disabledControlText: rgba(0, 0, 0, 0.2);
$macOS-grid: rgba(204, 204, 204, 1);
$macOS-headerText: rgba(0, 0, 0, 0.8);
$macOS-highlight: rgba(255, 255, 255, 1);
$macOS-labelColor: rgba(0, 0, 0, 0.8);
$macOS-linkColor: rgba(0, 104, 218, 1);
$macOS-placeholderTextColor: rgba(0, 0, 0, 0.2);
$macOS-quaternaryLabelColor: rgba(0, 0, 0, 0.1);
$macOS-secondaryLabelColor: rgba(0, 0 ,0, 0.5);
$macOS-selectedContentBackgroundColor: rgba(0, 99, 225, 1);
$macOS-selectedControlColor: rgba(179, 215, 255, 1);
$macOS-selectedControlTextColor: rgba(0, 0, 0, 0.8);
$macOS-selectedMenuItemTextColor: rgba(255, 255, 255, 1);
$macOS-selectedTextBackgroundColor: rgba(179, 215, 255, 1);
$macOS-selectedTextColor: rgba(0, 0, 0, 1);
$macOS-separatorColor: rgba(0, 0, 0, 0.1);
$macOS-shadowColor: rgba(0, 0, 0, 1);
$macOS-tertiaryLabelColor: rgba(0, 0, 0, 0.2);
$macOS-textBackgroundColor: rgba(255, 255, 255, 1);
$macOS-textColor: rgba(0, 0, 0, 1);
$macOS-underPageBackgroundColor: rgba(150, 150, 150, 0.9);
$macOS-unemphasizedSelectedContentBackgroundColor: rgba(220, 220, 220, 1);
$macOS-unemphasizedSelectedTextBackgroundColor: rgba(220, 220, 220, 1);
$macOS-unemphasizedSelectedTextColor: rgba(0, 0, 0, 1);
$macOS-windowBackgroundColor: rgba(236, 236, 236, 1);
$macOS-windowFrameTextColor: rgba(0, 0, 0, 0.8);
```

```scss
// iOS-colors-dark.scss
$iOS-systemBlue-dark: rgba(10, 132, 255, 1);
$iOS-systemGreen-dark: rgba(48, 209, 88, 1);
$iOS-systemIndigo-dark: rgba(94, 92, 230, 1);
$iOS-systemOrange-dark: rgba(255, 159, 10, 1);
$iOS-systemPink-dark: rgba(255, 55, 95, 1);
$iOS-systemPurple-dark: rgba(191, 90, 242, 1);
$iOS-systemRed-dark: rgba(255, 69, 58, 1);
$iOS-systemTeal-dark: rgba(100, 210, 255, 1);
$iOS-systemYellow-dark: rgba(255, 214, 10, 1);
$iOS-systemGray-dark: rgba(142, 142, 147, 1);
$iOS-systemGray2-dark: rgba(99, 99, 102, 1);
$iOS-systemGray3-dark: rgba(72, 72, 74, 1);
$iOS-systemGray4-dark: rgba(58, 58, 60, 1);
$iOS-systemGray5-dark: rgba(44, 44, 46, 1);
$iOS-systemGray6-dark: rgba(28, 28, 30, 1);
$iOS-label-dark: rgba(255, 255, 255, 1);
$iOS-secondaryLabel-dark: rgba(235, 235, 245, 0.6);
$iOS-tertiaryLabel-dark: rgba(235, 235, 245, 0.3);
$iOS-quaternaryLabel-dark: rgba(235, 235, 245, 0.2);
$iOS-placeholderText-dark: rgba(235, 235, 245, 0.3);
$iOS-separator-dark: rgba(84, 84, 88, 0.3);
$iOS-opaqueSeparator-dark: rgba(56, 56, 58, 1);
$iOS-link-dark: rgba(9, 132, 255, 1);
```

```scss
// iOS-colors-light.scss
$iOS-systemBlue: rgba(0, 122, 255, 1);
$iOS-systemGreen: rgba(52, 199, 89, 1);
$iOS-systemIndigo: rgba(88, 86, 214, 1);
$iOS-systemOrange: rgba(255, 149, 0, 1);
$iOS-systemPink: rgba(255, 45, 85, 1);
$iOS-systemPurple: rgba(175, 82, 222, 1);
$iOS-systemRed: rgba(255, 59, 48, 1);
$iOS-systemTeal: rgba(90, 200, 250, 1);
$iOS-systemYellow: rgba(255, 204, 0, 1);
$iOS-systemGray: rgba(142, 142, 147, 1);
$iOS-systemGray2: rgba(174, 174, 178, 1);
$iOS-systemGray3: rgba(199, 199, 204, 1);
$iOS-systemGray4: rgba(209, 209, 214, 1);
$iOS-systemGray5: rgba(229, 229, 234, 1);
$iOS-systemGray6: rgba(242, 242, 247, 1);
$iOS-label: rgba(0, 0, 0, 1);
$iOS-secondaryLabel: rgba(60, 60, 67, 0.6);
$iOS-tertiaryLabel: rgba(60, 60, 67, 0.3);
$iOS-quaternaryLabel: rgba(60, 60, 67, 0.2);
$iOS-placeholderText: rgba(60, 60, 67, 0.3);
$iOS-separator: rgba(60, 60, 67, 0.3);
$iOS-opaqueSeparator: rgba(198, 198, 200, 1);
$iOS-link: rgba(0, 122, 255, 1);
```