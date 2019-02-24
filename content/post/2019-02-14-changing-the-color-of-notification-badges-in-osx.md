---
title: Changing the color of notification badges in OSX
author: John-Henry
date: '2019-02-14'
slug: changing-the-color-of-notification-badges-in-osx
categories:
  - Blog
tags:
  - Mac
  - Notifications
  - Customization
subtitle: ''
description: 'Notifications don't need to be so urgent'
image: "img/white-slab.png"
draft: yes
---

Tristan Harris writes that after changed the notification label from blue to red, Facebook saw a dramatic uptick in useage. It is hypothesized because red is the color of urgency, the color red does a good job at hijacking our prehistoric brains into reacting quickly.

I'm not on Facebook. I am on OSX however, and I feel like I check my messages too much. I experimented turning off my message notifications full stop, but that didn't work for me. I found I started checking my messages even more because I was never sure if I had any or not. I recently changed my notifications to blue and I feel like this really helped.

Whenever I get I message it now looks like this:

![message](/img/messages/notification.png)

My guess is we'll see this feature becoming more common over time. For example, I see that Youtube uses blue dots now for subscription updates:

![](/post/2019-02-14-changing-the-color-of-notification-badges-in-osx_files/youtube-blue-dot.png)


I used [this guide](https://web.archive.org/web/20190214172228/https://forums.macrumors.com/threads/change-dock-icon-badges.1903323/) to make the change on my mac:

#### Get to the location

Open finder and click typle the key board shortcut `Shift ⌘ G` to open the "Go to folder" options. Go to:
```
/System/Library/CoreServices/Dock.app/Contents/Resources
```

#### Copy and edit the files
If you scroll down you'll find two relevant files. The `statuslabel.png` and the `statuslabel@2x.png`. Make a copy of these files onto your desktop and edit them however you wish. Personally, I had no experience editing images before this but I ended up using [Pixlr](https://pixlr.com/x/) a free online photo editing tool. Your goal should be to make both images the same (I didn't do this step so well, so my image are actually a little different, but in practice, I've never noticed any issues).

If you want to sidestep photo editing step, you can just take mine.

statuslabel.png
![statuslabel](/img/messages/statuslabel.png)


statuslabel@2x.png
![statuslabel2x](/img/messages/statuslabel@2x.png)


#### Move the originals out of the way.

We are almost done! Press "⌘ I "on one of the orignal files to and under the "Name & Extension" part see if it's editable.

If you are like me, you'll see they are not editable and this is where you find out your computer has System Integrity Protection (SIP) turned on. No worries!

(If you are unsure you if SIP is turned on, you can type `csrutil status` into terminal to double check. It needs to be disabled).

The way you can disable this feature is by restarting your computer while pressing ⌘R to put your computer into recovery mode. Once in recovery mode, you can click Utilities from the banner to open terminal and type the following commmand:

```
csrutil disable
```

Restart your computer, and you should be able to edit your files. (If my instructions are unclear you can see [here for a clearer guide](https://web.archive.org/web/20190214172910/https://www.howtogeek.com/230424/how-to-disable-system-integrity-protection-on-a-mac-and-why-you-shouldnt/).)


Great! Now go back to the same file location as described about and edit original the file names so that they are `statuslabel.backup.png` and `statuslabel@2x.backup.png` respectively. You will proably need to type in your password. Copy in your newly edited files, and give them the original titles statuslabel names. 

Voila! You've now changed the color of the notification badges on your computer!






