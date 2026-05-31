
# Hide and Seek

Writeup about TryHackme Hide and Seek Challenge Room

[Diffuculty: Easy] [Blue Team]

https://tryhackme.com/room/hfb1hideandseek


## Scenario

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekScenario.png)

From the question, the poems given us few hints:

- Time is on my side, always running like clockwork.
- A secret handshake gets me in every time.
- Whenever you set the stage, I make my entrance.
- I run with the big dogs, booting up alongside the system.
- I love welcome messages.

## Walkthrough

#### First Part

We can start investigation from the first part of the poem

`Time is on my side, always running like clockwork`

We start investigation for anything that related with time. I first start investigate into localtime, uptime, rtc files but no flag there.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Localtime.png)

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Uptime.png)

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1rtc.png)

Then I check the syslog file and found log that raise suspicious which is CMD execute echo string then pipe it to base64 to decode. This is common to deobfuscation a code.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Syslog.png)

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Syslog1.png)

Using Cyberchef to decode the obfuscated code, we can see it code for silent curl to download from a domain, but this is not the flag yet.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Decode.png)

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart1Decode1.png)

Once we decode the first part of the domain, we can see it the first part of the flag.

#### Second Part

Continue investigate the second part of the poems

`A secret handshake gets me in every time`

Hint here is handshake, we need to check anything about SSH since it associate with network authentication. At the home directory, we can see few suspicious directories. We start investigate the zeroday directory, in the directory have hidden ssh file. Upon checking the file, we can see another obfuscated string.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart2.png)

Once we decode the obfuscated string, we get the second part of the flag.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart2Decode.png)

#### Third Part

`Whenever you set the stage, I make my entrance`

The hint here is entrance, which mean it related to login. We check specter directory at home directory. We can see hidden files inside. Once we check the file `.bashrc`. We can see another obfuscated string.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart3.png)

We decode the obfuscated string, we can get the third part of the flag.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart3Decode.png)

#### Fourth Part

`I run with the big dogs, booting up alongside the system`

The fourth part of poem taking about booting up, we need to check anything that to do with service startup.

First I start checking the `/etc/passwd` but nothing there.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart4passwd.png)

Then I continue with checking the directories that handle services start which is `/lib/systemd/system` directory. Here we can see the file name exactly like in the scenario `cipher.service`, checking the file we can see another obfuscated string.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart4system.png)

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart4Decode.png)

Decode the string we found, we get the fourth flag.

#### Fifth Part

For the last part of the flag, it time to investigate the last part of the poems

`I love welcome messages`

The hint is about welcom messages. it must be realted with MOTD (Message Of The Day) file. Checking the directory `/etc/update.motd.d` we can see the last obfuscated string. 

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart5.png)

Decode the obfuscated string, we get the last part of the flag.

![App Screenshot](https://github.com/Muaz1425/Hide-and-Seek-room/blob/main/Images/HideAndSeekPart5Decode.png)

Combining all the parts of the flag, we can get the entire flag.

THM{y0u_g0t_3v3ryth1ng_d0wn}


