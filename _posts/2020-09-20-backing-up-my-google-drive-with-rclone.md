---
layout: post
title: "Backing up my google drive with rclone"
---
It's common knowledge that you should always try to back up your files in more than one place. For the longest time I've been backing up my files to only my google drive and while I think google drive is very reliable it did make me feel a little uneasy that my files were only stored in one place.

That is why yesterday I decided to start backing up my google drive to another cloud storage provider. There are a lot of tools out there that can do the job for you but they’re often paid or have some kind of limit to how many files you can transfer. That’s why I was happy to find a tool that was open source and could be used with a lot of different cloud providers.

Rclone is very well documented and I think anyone that’s able to use the command line should be able to figure it out. If you want to try it out for yourself you can head over to [https://rclone.org/](https://rclone.org/) and follow their very comprehensive documentation. If you’d like to know how I did it please keep reading. I did this on a windows machine but it should work just as well on any other platform.

First thing I did was download the rclone binary from the website and add it to my environment variables. This allowed me to access the program from powershell. You can simply type:

```bash
rclone
```

And you’ll get a list of available options.

The first thing you want to do is configure the services you’re going to use. You can do this by typing:

```bash
rclone config
```

Which will give you an interactive menu which leads you through the process of adding and removing services. To create a new service you simply type `n` and hit enter. You’ll give the services a name and choose which cloud storage provider you want to use. In my case google drive. After that you’ll get a prompt telling you that it’s recommended to configure your own google application client id. You can skip this step if you want and just accept the default but since I didn’t want to run into any limitations I opted to configure my own client id and secret so I wouldn’t run into any limitations. After that you can hit enter through the rest of the process and accept all the defaults.

Now that we’ve got google drive configured we can configure the other cloud storage provider which is pretty much the same process only this time I called the service stack and chose webdav as the storage type since this is what stack is using.

With both services configured we can start transferring files from one storage provider to the other. Because I want to move my files from google drive to stack I used the following command:

```bash
rclone sync googledrive: stack:googledrive -P
```

What this does is sync the root of my google drive to a folder called googledrive in my stack. The `-P` flag is used to show the current progress because it doesn’t show any progress by default. If you don’t specify a path it will transfer the files to the root of that service which is something you probably don’t want so make sure to specify a path where you want the files to be placed.

This made it really easy for me to transfer all my files from my google drive to my stack and it should also be really easy for me to automate in the future.
