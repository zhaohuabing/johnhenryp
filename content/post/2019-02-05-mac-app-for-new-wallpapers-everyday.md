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
image: "img/white-slab.png"
draft: no
---


I made a pretty cool app using Automator for mac that downloads wallpapers from the internet, deletes the old ones, and shuffles the gives me a new image everyday. I based the design off [this thread from 2011](https://www.reddit.com/r/apple/comments/gk0g7/here_is_how_to_create_an_automator_app_that_will/), but I also included a lot of improvements. It makes me very happy and impressed when I open my computer each day! In this post, I am going to start with screenshots of the app, and annotate the steps afterward:

<img src="/post/2019-02-05-mac-app-for-new-wallpapers-everyday_files/automator-1.png" alt="" width="80%"/>
<img src="/post/2019-02-05-mac-app-for-new-wallpapers-everyday_files/automator-2.png" alt="" width="80%"/>
<img src="/post/2019-02-05-mac-app-for-new-wallpapers-everyday_files/automator-3.png" alt="" width="80%"/>
<img src="/post/2019-02-05-mac-app-for-new-wallpapers-everyday_files/automator-4.png" alt="" width="80%"/>


Here is how it works:

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

3) Add a Run shell script block. I keep my images in a folder called Reddit_Wallpapers. The purpose of this block is to make sure this folder always exists. You will need to change the path to fit your own desired location.

```
if [ -d /Users/jhap/Documents/Desktops/Reddit_Wallpapers ]   
then 
    echo "dir present"
else
    echo "dir not present"
	  mkdir /Users/jhap/Documents/Desktops/Reddit_Wallpapers
	
fi
```

If you click the Run button of the upper right of the Automator window and then the results button of the shell block you will see whether or not the directory exists. If the folder does not exist yet, it will be created.


#### Empty out the folder
4) Get all the items with a "Get Specified Finder items" block. Add your folder.

5) Emptying the folder is an important to step to make sure your folder doesn't grow indefinitely. The touch command adds an empty file so that the rm will not fail even if the folder is empty. The osascript gets the path current desktop image so it won't be deleted. With the original version of this app I kept running into a problem where the original file would get deleted and then my laptop wouldn't know what to do, this circumvents that issue.

```
touch /Users/jhap/Documents/Desktops/Reddit_Wallpapers/filename.txt

osascript -e 'tell app "finder" to get posix path of (get desktop picture as alias)'
```

6) Add another Run Shell Script block. This one turns the path of the current desktop image into just the file name. Notice the "pass inputs as arguments" setting on the right.
```
basename "$@"
```

7) Add another Run Shell Script block. This block deletes everything except the current desktop image. I use the rm command because I didn't want my Trash to be filled with images. Notice the "pass inputs as arguments" setting on the right.
```
shopt -s extglob

cd /Users/jhap/Documents/Desktops/Reddit_Wallpapers/

rm -v !("$@")
```
#### Download the images


8) Add "Get specified URLS" block. I get my wallpapers from `feed://www.reddit.com/r/EarthPorn/.rss` but obviously you could change this step to fit any continuously updating source of images or even multiple.

9) Add a "Get Link URLs from Articles" block.

10) Add a "Filter URLs" block where any of the follow conditions are true. "Entire url" "ends with" .jpg, and one more line "Entire url" "ends with" .png. You could of course add more photo types in this step.

11) Download URLs to the folder. 


#### Keep only the high quality images

I think this is the step that makes my method really stand out.

12) Add a Filter Finder Items  "Any" where "Size" "is less than" 1 "MB". (Note I think you could pick a larger file size and still be pretty depending on how many sources you take your wallpapers from)

13) Add a "Run Shell Script" block which will delete all the small images. Notice the "pass inputs as arguments" setting on the right.
```
for f in "$@"
do
	rm "$f"
done
```


### Comprehension Check

If you click the run button in the upper right, the folder should propagate with new wallpapers. 

If you go to System Preferences > Desktop & Screen Saver > and click your given folder at this stage you should be almost good to go! Make sure you "Change Picture" button is toggled on. I like to keep mine on "change everyday", but that's just my personal preference.


At this place if you save your app, and click on it, the folder to should fill with new wallpapers. Apple will probably give you a bunch of things to approve. To stop this click approve a lot, or go to:

System Preference > Security & Privacy > Allow your new app to control your computer, and this won't happen again. 


#### Add auto updating

At this point you would want to automate and put your app on a timed schedule. I actually don't understand Chron too well, but I've been doing well with the freeware [CronniX](https://www.macupdate.com/app/mac/7486/cronnix) which does it for me. I have my app scheduled to work everyday at 8pm. 

I keep my app in my Applications folder. To run:
```
open /Applications/wallpaper_app.app
```

This should look like:
![cronnix](/img/wallpaper_updates/cronnix.png)

You will need to save and enter your password in order for this to work.




