---
title: Wazuh Project Challenges Pt.2
date: 25-01-2026
description: Write up on the second challenge in my Wazuh home lab
Author: Brandon
categories: [Security Projects]
tags: [wazuh, active response]
---

Now onto part 2 of this challenge. This write-up will cover the challenge: 'The "Active Response" (Automated Defense)'. This part of the challenge was fairly simple and straightforward, but I had some issues with the response not being triggered on the system. 

First I needed to check for the netsh.exe command to make sure it was defined, which it was:
```
    <command>
      <name>win-netsh-block</name>
      <executable>netsh.exe</executable>
      <timeout_allowed>yes</timeout_allowed>
    </command>
```
Then I needed to add the active response into the active responses section in the conf file. The first code block below is the initial command when I created it. It's just the basic response with the timeout period.
```xml
      <active-response>
       <command>netsh</command>
       <rules_id>100002</rules_id>
       <timeout>180</timeout>
      </active-response>
```
This code block is the final and more refined version. It ended up this way due to trying to test and focus down on why it might not be working on the system. The location and agent_id tags are there to specify which syste, it is being used on. Since I only have one Windows VM I needed to use the defined-agent value and then specify the specific agent, which is 001. But if I had more than one system I could use local as the location value which would run the command on the specific agent that generates the event.
```xml
      <active-response>
       <disabled>no</disabled>
       <command>netsh</command>
       <location>defined-agent</location>
       <agent_id>001</agent_id>
       <rules_id>100002</rules_id>
       <timeout>180</timeout>
      </active-response>
```
All that was needed after that was to run the Hydra attack and see the response. But I ran into a few issues, not sure if these were due to how Wazuh was setup when I installed it or just something I initially missed. I'll go through these in an rough order.

 1. My active response command was not running, I later found that I had it in the commented out initial command (whoops).
[image of commented out section](/assets/img/posts/wazuh/active_response_commented.png)
 2. I ran a few checks in the wazuh-logtest tool to check the command to see if it is firing, but it was not.
 3. After doing some asking Gemini for assistance and looking through the setup, I decided to try firewall-drop which is another command that does the same thing but works on linux and mac, alongside Windows. This command also didn't work due to the firewall-drop command not being on the Windows client in the Wazuh agent files.
 4. Finally I checked the in the /var/ossec/active-response/bin directory and saw there was no netsh file listed which means the netsh command in the conf folder had no way to run anything (This was the root of the issue). I got the information for the file and created it. It's possible that during my setup of Wazuh this was somehow not installed, but I'm not 100% sure on that.

After all of these steps above (which took a while to go through), I ran the Hydra attack and it detected it. I changed the value to four detection's within my initial rule, after that the initial rule fired my Kali attack client was blocked for 3 minutes (as a test) from accessing the Windows client.
[image of response success](/assets/img/posts/wazuh/active_response_success2.png)

Overall it was a success and I learned quite a bit from this. It was nice having to work through checking the rule, checking and making sure the command was correct, testing the rule, making sure my rule was correct, understanding what was on the Windows client, and how and where to check in case a main file is missing within the manager. Both the initial setup including the thought process of how you would setup this type of detection was probably the most important for me, but setting up the response and understanding how it works was fun and enjoyable to sort out.

Next challenge will be for Living off the land attacks (I completed it already and it was quite fun). Can't wait to share that.

Brandon
