---
layout: post
title: The future of CombineBot 4 - JS all the way
---

Hello everyone. I'm back again with some *very* big news for the future of CombineBot. First of all, I realize I haven't been adding much to the bots lately (besides fixing some AWFUL SQL bugs), and the reason for that will be discussed here. With that in mind, I'm sure must of you know about how a few months ago I started work on CombineJS, a recreation of CombineBot in discord.js. I made that for more reasons that I let one besides learning discord.js. To understand what I'm going to announce here, we need to go back to the beginning.

I started CombineBot, initially under the name UltraBot, which it had until August 2024, back in December 2023. At the time, my blog was down so I couldn't post about it. I made it on Bot Desginer for Discord, a platform where people use a GUI and the proprietary language BDScript to write commands. Yeah, it was pretty fucking bad. My initial vision was to add as many commands as possible, to make sure that whatever weird idea someone has, CombineBot can do it. But as I moved to Python in April 2024 and now to JavaScript, I realized just how bad that idea was. All I will end up with is a cluttered, poorly coded bot that is hard to manage, just for being there in rare scenerios with better options. Don't get me wrong, I still plan for the bot to have a good amount of features and commands, but not as much as originally intended.

So after I finish putting all that I want to into CombineJS, I will rebrand that as the new CombineBot and retire CombineBot python and archive its repo. If you don't want this, you are always free to host your own instance of CombineBot python or fork. But I won't be anymore. There is a lot of bad code in it and I think it's better to start fresh with a faster and more capable library.

Again, none of this will happen soon. I still need to import the SQL databases and test commands into CombineJS. But it will happen eventually. If you are reading this the day it comes out, I'll update this post later with a list of commands in the bot and which will be kept or removed. Small, rarely used commands (mostly external API commands) will be removed.

Command list:

/ping - kept

/helloworld - kept

/checklevel - kept

/about - kept

/toggleleveling - kept

/say - kept

/ephemeral - removed

/invite - removed (already part of /about)

/dadjoke - removed

/xkcd - kept

/xkcdrecent - kept

/dogpics - removed

/shakespeare - kept

/jojostand - removed

/jojocharacter - removed

/randomreddit - kept

/randombreed - removed

/urlshort - kept

/weather - kept

/httpanimal - kept

/pokedex - kept

/dictionary - kept

/fakeperson - removed

/bible - removed

/rss - kept

/demonlist - kept

/trivia - kept

/cattext - removed

/urbandictionary - kept

/qrcode - removed

All calculator commands - will be revamped

/mbtifinder - removed

/mbtilfunctions - removed

/cogfunctest - kept (code will be revamped)

/dndmodifier - removed

/enneagramtest - kept

/cowsay - kept

/randomword - removed

/coinflip - kept

/randomyt - removed (doesn't even work 99% of the time lmao)

/aaquote - removed

/edgeworthcube - I love it, but removed

/disc2mbti - kept

/mostmbti - removed

/mbtitest - kept

/mcjava - kept

/mcbedrock - kept

/userid - kept but will be merged into /userinfo

/ban - kept

/kick - kept

/purge - kept

/timeout - kept

/untimeout - kept

/createchannel - kept

/deletechannel - kept

/thread - kept

/channelid  - kept but will be merged into /channelinfo

/poll - kept

/addrole - kept

/createrole - kept

/removerole - kept

/roleinfo - kept

/rps - kept

/standbattle - will be revamped

/suntzuquotes - removed

/avatar - kept

/gettime - kept

/timestop - kept but will be revamped

/resume - kept but will be revamped

/userinfo - kept

/botinfo - will be merged into /about

/channelinfo - kept

/serverinfo - kept

/button - kept

/uuid - removed

/randomstring - removed

/unixtime - kept

/roleid - will  be merged into /roleinfo

/welcomescreen - kept

I will also be revamping the core (`combinebot.py`) to be not shit

I understand if you are angry or disappointed with me, but I assure you all that this *is* a step in the right direction, and hey, you can always host old versions yourself. I will put up another blog post soon about how to host your own instance of CombineBot. So if you haven't yet, be sure to invite CombineJS to your server from the CombineBot page above. 

Again, remember that I still care about this bot. My Rust learning has been getting in the way of my time but I have big ideas for future SQL integration and upgrades to the battle system. Maybe even a item shop...? You'll just have to find out!

Thank you for reading, and have a good day.

-CombineSoldier14
