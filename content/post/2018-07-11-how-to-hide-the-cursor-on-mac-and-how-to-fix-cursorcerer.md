---
title: How to Hide the Cursor on Mac and How to Fix Cursorcerer
author: John-Henry
date: '2018-07-11'
slug: how-to-hide-the-cursor-on-mac-and-how-to-fix-cursorcerer
categories:
  - Blog
tags:
  - Applescript
  - Productivity
  - Mac
  - Cursor
  - Cursorcerer
  - Tech
---

My mac used to hide the cursor by pressing F6 (fn F6 for those uninitiatied). Then one update, I don't know, Apple took that feature out? Anyway the best work around I found thus far was to download [Cursorcerer](http://doomlaser.com/cursorcerer-hide-your-cursor-at-will/), which is a freeware program which basically offers the same function with a couple extra features. 

However, I noticed if I don't use Cursorcerer for awhile it stops working. Because of this, I made an applescript that toggles it on and off which is much faster (and much less annoying) than going to my settings and toggling the Cursorcerer manually. For me, this always fixes the issue. I assigned this applescript to the hotkey Option + F6, which I find very intuitive. It is posted below:


```
tell application "System Preferences"
	activate
	set current pane to pane "Cursorcerer"
	delay 0.3
	tell application "System Events" to tell process "System Preferences"
		click the checkbox "Always show cursor if moved" of window "Cursorcerer"
		delay 0.3
		click the checkbox "Always show cursor if moved" of window "Cursorcerer"
	end tell
end tell
tell application "System Preferences" to quit

```

If your  F6 key still hides your cursor then you probably don't need Cursorcerer. If your Cursorcerer always works, then you probably don't need this applescript. But this is the solution that worked for me.