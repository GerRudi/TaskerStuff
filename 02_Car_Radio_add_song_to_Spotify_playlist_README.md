# Description

When pressing "Repeat" or "Shuffle" on my car entertainment system, it automatically adds the currently playing song to a specific Spotify playlist. 


# Requirements (Add-ons)
- This Task requires the [AutoWeb](https://play.google.com/store/apps/details?id=com.joaomgcd.autoweb&hl=en) addon to access the Spotify API (Paid App from the Playstore) 
- This Profile requires [permissions to read system logs](https://tasker.joaoapps.com/userguide/en/help/ah_read_logs_grant.html)

This setup is working with a Composition Media VW Infotainment system (with firmware) from 2014 in combination with my Android 10 device (Android One Nokia 7 Plus) connected via Bluetooth. 
It might work with other devices without any further setup - or not at all. 

# Files (Click for Tasker import)

### Profile
- **[01_BT_VW_connect.prf:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Profile%3A01_BT+VW+connect)** Event when connecting to your Bluetooth radio - **configure the MAC Address of your device**!
- **[02_Do_Spotify.prf:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Profile%3A02_Do+Spotify)** Event to capture button press
- **[03_Cooldown.prf:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Profile%3A03_Cooldown)** Event to trigger cooldown

### Tasks (the profile already includes the tasks!)

- **[01_BT_VW_Start.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3A01_BT+VW+Start)** Tells tasker, that you're now connected to the bt device
- **[01_BT_VW_End.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3A01_BT+VW+End)** Tells tasker that you're no longer connected to the bt device
- **[02_Do_Spotify.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3A02_Do+Spotify)** Gets currently playing track and adds it to the playlist - **configure your Playlist URI**!
- **[03_Timeout.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3A03_Timeout)** Enables the cooldown so that the task wont run more than once for the same button press



# Idea, Motivation, Challenges and Lessons Learned

Setting this up took longer than expected. 

My original motivation: 

When driving long distances, 
I like to listen to my "Discover Weekly" playlist or other random playlists on Spotify hoping to find new songs that I enjoy. 

However, I had no way to "save" these songs anywhere without using my phone. 
My car radio only offers PLAY, NEXT, REPEAT and SHUFFLE buttons, but I never use the Shuffle / Repeat buttons. 
So I was wondering if I could remap their functionality in some way. 

At first I was hoping these buttons are just simple KeyInput events and tried to capture them with AutoInput - no success. 
Then I analyzed the Bluetooth Android log (btsnoop_hci.log) with Wireshark to understand what's going on. 
This was fun and I learned a bit about how some Bluetooth protocols work but tasker has no way to capture these incoming Bluetooth commands. 

Some people on the /r/Tasker Subreddit then told me that Tasker "recently" got an update and is now able to read system logs. 
Setting up a logcat task can be a bit tricky using only the Tasker App so I first captured the logs using MatLog Libre from F-Droid. 

After that it was a bit of trial and error to find a reliable Logcat entry (as of writing this, there's also no way to use wildcards for the logcat entries, which also made things harder). 
I found an entry that was always written when I pressed the repeat/shuffle button but this lead to the next (and final) problem. 
This entry also gets written immediately after connecting to the radio unit and can also be triggered several times while only pressing the button once. 
Therefore I had to add some cooldowns using variables and the script was finally working great! 


# Possible enhancements
You could add a condition to check if Spotify is actually currently running. 
That way you can actually use the buttons in combination with other music players without any possible side effects. 

# Tasker descriptions

```
Profile: 01_BT VW connect (7)
    	Restore: no
    	State: BT Connected [ Name:* Address:33:33:33:33:33:33 ]
    Enter: 01_BT VW Start (37)
    	A1: Variable Set [ Name:%vActive To:4 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ] 
    
    Exit: 01_BT VW End (38)
    	A1: Variable Set [ Name:%vActive To:2 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ]
```

```
Profile: 02_Do Spotify (28)
    	Restore: no
    	Event: Logcat Entry [ Output Variables:* Component:Avrcp_ext Filter:Cmd = 6on index = 0new entrytrue ]
    Enter: 02_Do Spotify (39)
    	A1: If [ %vActive < 1 ]
    	A2: Variable Set [ Name:%vActive To:4 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ] 
    	A3: AutoWeb Web Service [ Configuration:API: Spotify
    API Action: Get Currently Playing Track
    Market: US Timeout (Seconds):120 ] 
    	A4: AutoWeb Web Service [ Configuration:API: Spotify
    API Action: Add Tracks to a Playlist
    Market: US
    Playlist Id: 1337 Timeout (Seconds):120 ]
```

```
Profile: 03_Cooldown (30)
    	Restore: no
    	Event: Variable Set [ Variable:%vActive Value:4 User Variables Only:Off ]
    Enter: 03_Timeout (40)
    	A1: If [ %vRunning neq 1 ]
    	A2: Variable Set [ Name:%vRunning To:1 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ] 
    	A3: Wait [ MS:0 Seconds:10 Minutes:0 Hours:0 Days:0 ] 
    	A4: Variable Set [ Name:%vRunning To:0 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ] 
    	A5: If [ %vActive eq 4 ]
    	A6: Variable Set [ Name:%vActive To:0 Recurse Variables:Off Do Maths:Off Append:Off Max Rounding Digits:3 ]
```








