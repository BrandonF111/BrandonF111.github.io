---
title: Wazuh Project Challenges Pt.3
date: 10-02-2026
description: Write up on the third challenge in my Wazuh home lab
Author: Brandon
categories: [Security Projects]
tags: [wazuh, detection engineering]
---

This write up will cover the challenge of detecting "Living off the Land" (LotL) Attacks. Honestly I've never really done much with these types of attacks before. I do recall a TryHackMe lesson on sysmon and some of these native processes, but that was a while back. As I learned more about these attacks I ended up find a page that had a list LotL binaries and it is interesting to see all the ways that native things in Windows can be used for malicious purposes. [LOLBAS](https://lolbas-project.github.io/#)

## **Detecting suspicious powershell usage**
I started off this challenge with trying to detect suspicious powershell usage. I setup a payload in metasploit and just copied the command into powershell, which Windows ended up blocking thankfully. After disabling the protection I ran the command, which was an encoded powershell command. Wazuh did pick up since it has an internal sysmon rule for that, but for the sake of learning I created my own rule.
![lotl powershell rule](assets/img/posts/wazuh/lotl_rule.png)
After that I ran the command again and it was detected with my custom rule.
![lotl powershell detection](assets/img/posts/wazuh/lotl_detection.png)
The neat part is that within the rule detection it shows you the entire command that was ran. Taking the encoded command I went into [cyberchef](https://gchq.github.io/CyberChef/) and put it in and decoded it.
![decoded command](assets/img/posts/wazuh/lotl_decoded.png)
Thankfully Windows already blocks the usage of a powershell command that is encoded, but in case that was ever to get removed this rule would detect it.

## **Detecting Certutil usage**
Originally the powershell detection was going to be the only one I did, but the AI agent I was working with added a few more so I decided why not do them and learn some more. This challenge was trying to detect certutil being used. Initially Windows Security already blocked it, which was good to see. After it was disabled I ran the command again.
[certutil powershell command](assets/img/posts/wazuh/lotl_certutil_command.png)
Wazuh did have a detection built in for it, but I still ended up creating my command to test and I feel that the wording was a bit better
![certutil rule](assets/img/posts/wazuh/lotl_certutil_rule.png)
After that I ran it again and got the detection. The picture shows the internal Wazuh rule and my custom rule above it.
![certutil detection](assets/img/posts/wazuh/lotl_certutil_detect.png)
And then here is the detection data which shows what was downloaded using certutil
![wazuh certutil detection data](assets/img/posts/wazuh/lotl_certutil_data.png)

## **Detecting suspicious schtasks usage**

The finally challenge related to LotL was detecting schtasks being used for a suspicious repeated task. I was initally using the sysmon file from the Wazuh site but it was not detailed enough to detect the schtasks usage. I ended up switching to the SwiftonSecurity sysmon file found [here](https://github.com/SwiftOnSecurity/sysmon-config?tab=readme-ov-file). After that there was a long process to find where the sysmon event for the process. It seems that there was no specific rule for schtasks when it was used. Here is a summary of how I was able to locate the system event with Wazuh:
**1. Verified Sysmon Logging**
-   Confirmed Sysmon was capturing the event (Event ID 1 - Process Creation) in Windows Event Viewer
-   The event contained key fields: `Image`, `CommandLine`, `ParentImage`, etc.

**2. Identified the Detection Gap**
-   While existing Wazuh rules detected related activity (rule 92052 for cmd.exe spawning), no rule specifically caught the suspicious `schtasks` creation
-   Searched Wazuh alerts and found no existing detection for this persistence technique

**3. Analyzed Wazuh Rule Structure**
-   Located the base Sysmon rules in `/var/ossec/ruleset/rules/0330-sysmon_rules.xml`
-   Found that rule **184665** creates the `sysmon_event1` group for Sysmon Event ID 1
-   Examined existing detection rules in `/var/ossec/ruleset/rules/0800-sysmon_id_1.xml` (like rule 92052) to understand syntax and field names

After all of that (which took quite a while to hunt down) I was able to create the rule. My regex is still bad so I got some help with that. The rule is looking for the process creation event from Sysmon, then looking for schtasks regardless of case, also if it contains the /create flag, and finally if it matches for minute or hourly creation.
![schtasks rule](assets/img/posts/wazuh/lotl_schtask_rule.png)

The command was run again and detected by my custom rule!
![schtasks rule detection](assets/img/posts/wazuh/lotl_schtask_wazuh.png)
![schtasks detection data](assets/img/posts/wazuh/lotl_schtask_wazuh_details.png)

This challenge was quite interesting and I learned a lot. It was interesting to learn about Living of the Land attacks and how so many native binaries and commands can be used in a malicious manner. Also nice to see that the default Windows security system is actually pretty good. I definitely have to look back through this to get a good understanding of some of the xml fields within my last rule. But overall it was a great learning experience and it will be fun to look back into this and continue to learn more.

Brandon
