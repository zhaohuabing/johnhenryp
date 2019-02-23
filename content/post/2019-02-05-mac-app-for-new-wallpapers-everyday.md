---
title: Mac App for New Wallpapers Everyday
author: John-Henry
date: '2019-02-05'
slug: mac-app-for-new-wallpapers-everyday
categories:
  - Blog
tags:
  - Applescript
  - Productivity
  - Mac
subtitle: ''
description: 'Automatically download wallpapers from the Internet'
image: ''
draft: yes
---


I made a pretty cool app using Automator for mac that downloads wallpapers from the internet, deletes the old ones, and shuffles the gives me a new image everyday. I based the design off [this thread from 2011](https://www.reddit.com/r/apple/comments/gk0g7/here_is_how_to_create_an_automator_app_that_will/). It makes me very happy and impressed when I open my computer each day! In this post I am going to walk you through how to make it, and I am going to post screenshots of the final product at the bottom.



1) Open Automator -> New Document -> Application


### Make sure there is an Internet connection

2) Add a run Applescript block and add this code. The purpose of the step is to make sure the app is connected to the Internet and not to run otherwise
```
try
	do shell script "curl www.google.com"
on error
	error number -128
end try
```


#### Create a folder for the downloaded images to go

3) Add a Run shell script block. I keep my images in a folder in a folder called in a folder called Reddit_Wallpapers. The purpose of this block is to make sure this folder always exists. You will need to change the path to fit your own desired location.

```
if [ -d /Users/jhap/Documents/Desktops/Reddit_Wallpapers ]   
then 
    echo "dir present"
else
    echo "dir not present"
	  mkdir /Users/jhap/Documents/Desktops/Reddit_Wallpapers
	
fi
```

If you click the Run button of the upper right of the Automator window and then the results button of the shell block you will see whether or not the directory exists. If you the folder does not exist yet, it will be created.


#### Empty out the folder

4) Emptying the folder is an important to step to make sure your folder doesn't grow indefinitely. The touch command adds an empty file so that the rm will not fail even if the folder is empty. I use the rm command because I ddin't want my trash can filled with images

```
touch /Users/jhap/Documents/Desktops/Reddit_Wallpapers/empty.txt

rm /Users/jhap/Documents/Desktops/Reddit_Wallpapers/*
```


#### Download the images


5) Add "Get specified URLS" block. I get my wallpapers from feed://www.reddit.com/r/EarthPorn/.rss but obviously you could change this step to fit any continuously updating source of images or even multiple.

6) Add a "Get Link URLs from Articles" block.

7) Add a "Filter URLs" block where any of the follow conditions are true. "Entire url" "ends with" .jpg, and one more line "Entire url" "ends with" .png. You could of course add more photo types in this step.

8) Download URLs


#### Keep only the high quality images

I think this is the step that makes my method really stand out.

9) Add a Filter Finder Items  "Any" where "Size" "is less than" 1 "MB". (Note I think you could pick a larger file size and still be pretty depending on how many sources you take your wallpapers from)

10) Add a "Run Shell Script" block which will delete the small images
```
for f in "$@"
do
	rm "$f"
done
```


### Comprehension Check

If you click the run button in the upper right, the folder should propagate with new wallpapers. 

If you go to System Preferences > Desktop & Screen Saver > and click your given folder at this stage you should be almost good to go! Make sure you "Change Picture" button is toggled on. I like to keep mine on "change everyday", but that's just my personal preference.


#### Refresh the Change Wallpaper Button


With the original version of this app I kept running into a problem where the original file would get deleted and then my laptop wouldn't know what to do when I opened it the next day. The work around I use is to just toggle the change wallpapers button. There is probably a more elegant work around, but I don't know what it is.


11) Add a "Run Applescript" block

```
tell application "System Preferences"
	activate
	set current pane to pane "com.apple.preference.desktopscreeneffect"
end tell
delay .5
tell application "System Events"
	tell process "System Preferences"
		tell window "Desktop & Screen Saver"
			delay 0.3
			tell tab group 1
				click checkbox "Change picture:"
				delay 0.3
				click checkbox "Change picture:"
			end tell
		end tell
	end tell
end tell
delay 0.3
tell application "System Preferences" to quit
```

At this place if you save your app, and click on it, the folder to should fill with new wallpapers. Apple will probably give you a bunch of things to approve but 

System Preference > Security & Privacy > Allow your new app to control your computer this won't happen again. 


#### Add auto updating

At this point you would want to automate . I actually don't understand Chron too well, but I've been doing well with the freeware [CronniX](https://www.macupdate.com/app/mac/7486/cronnix). You will want to schedule the app to (I do mine everyday at 8pm). 

I keep my app in my Applications folder. To run:
```
open /Applications/wallpaper_app.app
```

This should look like:
![cronnix](/img/wallpaper_updates/cronnix.png)

You will need to save and enter your password in order for this to work.

#### Final Product

If you've been following along you should end up with something like:

![automator1](/img/wallpaper_updates/automator1.png)
![automator2](/img/wallpaper_updates/automator2.png)
![automator3](/img/wallpaper_updates/automator3.png)

