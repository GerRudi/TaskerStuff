# Description

When launching a specific app, the Task automatically enables Do not Disturb mode until the app is no longer running in the foreground. 
This is really helpful when you try to concentrate. 

In this example, I'm using it together with Duolingo (learning languages) and Simply Piano (learning piano). 


# Requirements (Add-ons)
None

# Files (Click for Tasker import)

### Profile
**[PerApp_Profile.prf:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Profile%3APerApp_Profile)** Defines the apps that should use this feature

### Tasks (the profile already includes the tasks!)

**[Set_DnD.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3ASet+DnD)** Enables Do not Disturb

**[Unset_DnD.tsk:](https://taskernet.com/shares/?user=AS35m8nv%2F9kVVjaWNhsxWPMIrYDvleGnAAXvNLF0YGZMaXdHHvDCymFLorNzaH%2BXlk0dBJup&id=Task%3AUnset+DnD)** Disables Do not Disturb


# Tasker Description
```
Profile: AutoDnD (2)
    	Restore: no
    	Application: Duolingo or SimplyPiano
    Enter: Set DnD (3)
    	A1: Do Not Disturb [ Mode:Alarms ] 
    
    Exit: Unset DnD (5)
    	A1: Do Not Disturb [ Mode:Allow All ]
```
