---
layout: post
title: "How to schedule a task for rclone in windows"
---
I recently wrote a blog post about [how I made a backup of my google drive with rclone]({% post_url 2020-09-24-how-to-schedule-a-task-for-rclone-in-windows %}). Whenever I want to make a backup however I still have to do it manually by entering the rclone command in my terminal. Because I want the backups to happen regularly without me having to think about it I decided to automate it. This blog post will be about how I scheduled a task in windows to back up my google drive using rclone every hour my computer is on.

This was my first time using the windows task scheduler but luckily there are enough resources online which explain how to use it. It’s pretty straightforward and you can look at existing tasks to get an idea how they work and what the best settings are. The most important tabs are general, triggers and actions the rest you can just leave as is.

The first thing you want to do is create a new task by clicking “create task”, don’t click on “create basic task” because it won’t give you all the options you need. After that give your task a name and optionally a description. I changed “configure for” to windows 10 but I don’t think it really matters. Now you can switch to the triggers tab and create a new trigger. I configured mine to run every day and repeat every hour for the duration of a day but you can also run it only one time and let it repeat indefinitely although I’m not sure how that will work out. With that done we can switch to the actions tab which is arguably the most important one since we define which program we’ll want to run and with what parameters. Create a new action and under program/script browse to the rclone executable. Next add the arguments you want the program to receive when it’s executed. In my case this is:

```bash
sync googledrive: stack:googledrive
```

because I want my google drive to be synced to the googledrive directory in my stack. After that you can exit out of both windows by clicking OK.

To see if our task actually works we should give it a test run. Select the task from the list and click run in the right column. This should open a console window and run whatever program you selected, in my case rclone. After it’s done it should close again and if you refresh the list you should see when the task last ran and what the exit code was in the “last run result” column. If everything went well you can leave your task as is otherwise you should edit your task till you get the desired result. It took me a few iterations until I had configured everything correctly.

As you’ve probably noticed every time it runs your task it will open a terminal. While this may be desirable when you’re testing if everything works it’s not very nice if it pops up every time while you’re doing something else. The best and easiest solution I’ve found for this so far is to go to the general tab of your task and selecting the option “run whether user is logged on or not” and ticking the “do not store password” box. This somehow hides the program you're running but I’m not quite sure how. If you want to make sure your task is still running your program after making this change you can open the task manager and look for the program after you started the task. If everything is working correctly it should still show up.

You should be all set now. I’ve been running this task for 4 days now and haven’t had any issues yet. All my files are being synced and show up in my other cloud storage.
