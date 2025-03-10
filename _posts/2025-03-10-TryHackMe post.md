---
title: Page Updates for 2025
date: 2025-03-3
description: TryHackMe Security Engineering path interesting points
---

## TryHackMe - Security Engineering Path
### Identity and Access Management room write-up

This is going to be my first write-up as I go through the Security Engineer path on TryHackMe. It won't be a write-up in the traditional sense, but I will be pointing out things I've learned and thought were interesting while going through the room. Also how some of this information relates to the work I do. So let's get started!

### IAAA Model
The IAAA model refers to Identification, Authentication, Authorization, and Accountability. Learning about this model was interesting for me since it wasn't something I really though of when it came to relating to confidentiality, integrity, and availability. But after going through the sections I definitely made sense. 

Within the **Authentication** section it was nice to remember the rule of something you know/have/are. When trying to authenticate someone we don't want to have two of the same mechanisms. Have two of Something you know would allow an attacker to gain access by just obtaining two passwords or two pins from a user. While adding in one other authentication methods would require the attacker to obtain the user's physical security key, or even the person themselves. 

At my current job we follow this through something you know and have. We have a pin and a security key system, which works great. For any user working remotely an attacker would basically have to go to their home in order to obtain that key, which also can be removed if there is any breach in security.
### Attacks on Authentication
Another interesting thing I found was in the **Attacks Against Authentication** section. It described a replay attack, which is when an attacker can replay the response of a users encrypted password and since that doesn't change they would be able to access a system. The mitigation for this was to make the challenge response unique. The example used described encrypting the date and time and sending that with the password. The response would only be valid for a limited time but the server could verify the user and allow them access. Though this brought up some additional questions in my head, I'll have to do a more deep dive on how that solution can work.

### Access Control Models
The section for Access control models was short, but I found it very interesting once I realized how it tied into most systems we encounter at work, or even in our personal lives. It described three common access control models:
1.  Discretionary Access Control (DAC)
2.  Role-Based Access Control (RBAC)
3.  Mandatory Access Control (MAC)

DAC is where the resource owner will add users with permissions to see either a file or folder. Since users are specifically added, it is at the discretion of the resource owner as to who can see the information. I realized that we do this all the time in our personal lives. Whether sharing access to pictures, sharing access to streaming services, and even wifi at home when we have guests over. All of these things use DAC in some way, and that I found interesting.

RBAC is where users are assigned roles, and then access is provided based on the roles. Almost all of us, including me interact with this on a daily basis at work. Even when helping users with IT issues at my job, there are certain things I can't access on my end since I don't have the role/permission that allows access. 

Those were the sections in the IAM room that I found interesting. It was great to learning about these things in further detail, and it's nice to be able to relate them even within the work I do. Going through this room made me realize how much IAM is involved with a lot of people's day to day work, which makes it a fairly important security aspect to have implemented correctly.

Can't wait to continue learning more down this path and sharing my thoughts.
> Written with [StackEdit](https://stackedit.io/).
