# Description

When pressing "Repeat" or "Shuffle" on my car entertainment system, it automatically adds the currently playing song to a specific Spotify playlist. 


# Requirements (Add-ons)
- This Task requires the [AutoWeb](https://play.google.com/store/apps/details?id=com.joaomgcd.autoweb&hl=en) addon to access the Spotify API (Paid App from the Playstore) 
- This Profile requires [permissions to read system logs](https://tasker.joaoapps.com/userguide/en/help/ah_read_logs_grant.html)

This setup is working with a Composition Media VW Infotainment system (with firmware) from 2014 in combination with my Android 10 devices (Android One Nokia 7 Plus) connected via Bluetooth. 
It might work with other devices without any further setup - or not at all. 

# Files (Click for Tasker import)

### Profile
**[tbd.prf:](tbd)** tbd

### Tasks

**[tbd.tsk:](tbd)** tbd

**[tbd.tsk:](tbd)** tbd

# Idea, Motivation, Challenges and Lessons Learned

Setting this up took longer than expected. 

My original motivation: 

When driving long distances, 
I like to listen to my "Discover Weekly" playlist or other random playlists on Spotify hoping to find new songs that I enjoy. 

However, I had no way to "save" these songs anywhere without using my phone. 
My car radio only offers PLAY, NEXT, REPEAT and SHUFFLE buttons, but I never use the Shuffle / Repeat buttons. 
So I was wondering if I could remap their functionality in some way. 

At first I was hoping these buttons are just simple KeyInput events and tried to capture them with AutoInput - no success. 
Then I analyzed the Bluetooth Android log (btsnoop_hci.log) to understand what's going on. 
This was fun and I learned a bit about how some Bluetooth protocols work but tasker has no way to capture these incoming bluetooth commands. 

Some people on the /r/Tasker Subreddit then told me that Tasker "recently" got an update and is now able to read system logs. 
To setup a logcat task can be a bit tricky using only the Tasker App so I first captured the logs using MatLog Libre from F-Droid. 

After that it was a bit of trial and error to find a reliable Logcat entry (as of writing this, there's also no way to use wildcards for the logcat entries, which also made things harder). 
I found an entry that was always written when I pressed the repeat/shuffle button but this lead to the next (and last) problem. 
This entry also gets written immediately after connecting to the radio unit and can also be triggered several times while only pressing the button once. 
Therefore I had to add some cooldowns using variables and the script was finally working great! 


# Possible enhancements
You could add a check if Spotify is actually currently running. 
That way you can actually use the buttons in combination with other music players without any issues. 
