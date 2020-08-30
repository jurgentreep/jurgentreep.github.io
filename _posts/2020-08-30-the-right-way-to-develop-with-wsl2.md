---
layout: post
title: "The righ way to develop using WSL2"
---
I recently tried to set up a react app using WSL2 but I couldn't get it to work. The dependencies took really long to install or failed to install at all. I previously set up a react app using WSL1 without any problems so I was wondering why it wouldn't work under WSL2. When I finally did manage to install the required dependencies and got it to load in the browser it didn’t automatically update the app after I made a change to the code.

At this point I was so frustrated with the development experience that I installed Fedora next to Windows and developed the app there without any problems. I must say that developing on Fedora is a really nice experience were it not for the few things that really grind my gears. For example it’s not possible to set the scroll speed of your mouse and the US International keyboard just doesn’t work like I’m used to. For this reason I’m still using Windows as my main operating system and that’s also why it would be really nice to get WSL2 to work properly.

Anyway today, a few weeks later, I decided to give it another go. I read somewhere that WSL2 now uses its own hard drive and that you shouldn’t work on the mounted Windows drive like I used to do with WSL1. This wouldn’t be the first thing you’d think of doing because whenever you start WSL2 you get dropped in the mounted Windows drive and not your home directory.

With all this in mind the first thing I did was change my directory to the real home directory after I logged in. After that I did what I usually do when creating a react app and lo and behold it worked like a charm on the first try! On top of that the dependencies installed way faster and whenever I made a change to my code it would recompile faster than ever before. 

I’m glad I found out what the issue was and I’m looking forward to developing more projects using WSL2. The integration with Docker and Visual Studio Code is great and makes for a really nice developer experience.


