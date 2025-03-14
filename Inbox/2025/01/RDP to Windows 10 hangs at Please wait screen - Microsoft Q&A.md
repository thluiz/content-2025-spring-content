---
created: 2025-01-30T08:29:17 (UTC -03:00)
tags: []
source: https://learn.microsoft.com/en-us/answers/questions/451406/rdp-to-windows-10-hangs-at-please-wait-screen
author: 
---

# RDP to Windows 10 hangs at Please wait screen - Microsoft Q&A

> ## Excerpt
> RDP to Windows 10 hangs at the 'Please wait'  screen forever until rebooted.  
In our organisation, we are often seeing this issue on domain-joined systems & VMs with Windows 10 OS   
This happens when users are trying to log in with their domain…

---
RDP to Windows 10 hangs at the 'Please wait' screen forever until rebooted.

In our organisation, we are often seeing this issue on domain-joined systems & VMs with Windows 10 OS

This happens when users are trying to log in with their domain accounts. When the issue is reported, we IT team can log in with our Domain ADM accounts and reboot the system/VM. The user will be able to log in now, but the issue comes back again after a day or two. These systems/VMs are updated with the latest patches.

Even after running SFC Scan, the issue remains.

## 35 answers

1.  I ran into the same issue as a end-user with limited security. I was able to resolve my issue as an end-user:
    
    1.  in Powershell, I ran "query user /server:<SERVERNAME>". The server name was my remote desktop at the office.
    2.  I made note of my session id that was stuck.
    3.  I then ran "reset session <SESSION ID> /server:<SERVERNAME>".
    
    That did the trick for me. Anytime, I run into this problem I run this. I'm trying to make it a habit to always log off my session every time to prevent this from happening.
    
    If someone creates a temporary ps script for this, it would be interesting.
    
2.  ![](data:image/svg+xml, %3Csvg xmlns='http://www.w3.org/2000/svg' height='64' class='font-weight-bold' style='font: 600 30.11764705882353px "SegoeUI", Arial' width='64'%3E%3Ccircle fill='hsl(217.60000000000002, 70%, 31%)' cx='32' cy='32' r='32' /%3E%3Ctext x='50%25' y='55%25' dominant-baseline='middle' text-anchor='middle' fill='%23FFF' %3EE%3C/text%3E%3C/svg%3E)
    
    [Eric](https://learn.microsoft.com/en-us/users/na/?userid=6ee8b7f0-ad94-44d9-a2e4-ae71371bd3d7) 246 Reputation points
    
    Nov 19, 2021, 3:36 AM
    
    Hi MarlowSchlebusch-9296 and Marschos-8403,
    
    Sorry I didn't post this sooner. I wanted to see if MS would respond (nope).
    
    Windows PC - RDP Client mehtod- save the RDP connection to a ".rdp" file and edit the configuration through MS 'Notepad' to add a configuration line that will disable NLA login method and force Windows Login
    
    1.  Open MS RDP (Start->type "mstsc"->Enter) and type in the IP address you want to connect to
    2.  Click the "Options" button at the bottom and Click "Save" to save your connection as a ".rdp" file. Remember where you saved the file.
    3.  Click Start, type Notepad, Hit Enter
    4.  Click File, Open, traverse to the ".rdp" file you saved in 3 and open it.
    5.  Scroll to the last line and hit enter to add another line.
    6.  Add the following to turn of the Network Level Authentication: "enablecredsspsupport:i:0"
    7.  Click File, Save to save the ".rdp" file
    8.  Click Start, type "File Explorer"
    9.  Traverse to the directory which contains the ".rdp" file (do I need to provide instructions for this?)
    10.  Double Click the ".rdp" file
    11.  Connect, see Windows style login screen.
    12.  Enter your credentials and get past the "Please Wait" screen
    
    Note: After adding this configuration line, when you double click the ".rdp" file to connect, it will connect to the Windows PC and then display the login page where you will have to manually type in the User and Password. Unfortunately, it will save the User for next time you connect but not the Password. You'll have to enter it every time, but you will at least get past the "Please Wait" screen.
    
3.  Hi,
    
    This is how I have solved the problem:
    
    1.  Connect via RDC to a different account.
    2.  Then open Task Manager.
    3.  Navigate to users.
    4.  Locate the user account which has hanged i.e. you cannot connect to.
    5.  Right click on the user name and select reconnect.
    6.  Type in the password.
    7.  Get connected to the desired account.
    
    That worked for me on WIN10 via Android RDC.
    
4.  ![](data:image/svg+xml, %3Csvg xmlns='http://www.w3.org/2000/svg' height='64' class='font-weight-bold' style='font: 600 30.11764705882353px "SegoeUI", Arial' width='64'%3E%3Ccircle fill='hsl(217.60000000000002, 70%, 31%)' cx='32' cy='32' r='32' /%3E%3Ctext x='50%25' y='55%25' dominant-baseline='middle' text-anchor='middle' fill='%23FFF' %3EE%3C/text%3E%3C/svg%3E)
    
    [Eric](https://learn.microsoft.com/en-us/users/na/?userid=6ee8b7f0-ad94-44d9-a2e4-ae71371bd3d7) 246 Reputation points
    
    Jul 14, 2021, 11:39 AM
    
    Hello Leila MSFT,
    
    I have found new information that could lead to a solution of the issue if RDP getting stuck at "Please Wait" after connecting. Please determine why this alternate app allows a normal connection, even when MS RDP app gets stuck at "Please Wait":
    
    1.  Recreate issue- using RDP to connect to the destination pc gets stuck at "Please Wait", usually after connecting through a VPN and then putting the client laptop in sleep by closing the loud and reopening later (makes destination pc RDP service get stuck)
    2.  On an Android phone, use "Microsoft Remote Desktop" app to connect, it also gets stuck at "Please Wait".
    3.  From Google Play store, install "**aFree RDP**" which is a 3rd party RDP app. Setup the connection with the exact same user/ lpassword you used on MS RDP and connect to the destination pc. You will notice that the aFreeRDP connects as "Other User"and then enters the user/password credentials in there, and then it connects to the pc successfully with the desktop being shown as normal. You can use the pc as you would.
    4.  That's it. At this point if you disconnect and try MS RDP it is still stuck at "Please Wait" since it doesn't appear to use the "Other User" method to log in. But if you connect back using aFreeRDP it uses "Other User" which enters the credentials there and logs in perfectly.
    
    It seems when connecting to the session as **"Other User"** and entering the credentials there, the system creates a new session, whereas the official MS RDP is stuck using the old session which is somehow in an infinite loop wait state.
    
    MS PLEASE FIX IT
    
5.  The scope of the problem isn't limited to not being able to RDP in my experience. When the "Please Wait" RDP issue starts, other issues are introduced as well; logging in to the system locally and trying to "Run as administrator" for any program doesn't work as the UAC box never comes up. Curious if others have seen similar.
    

[Sign in to answer](https://learn.microsoft.com/en-us/answers/questions/451406/rdp-to-windows-10-hangs-at-please-wait-screen#)

### Question activity
