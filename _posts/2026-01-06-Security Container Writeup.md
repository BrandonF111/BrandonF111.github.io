---
title: Security Container Pipeline write-up
date: 2026-01-06
description: Write-up on my security container pipeline project
Author: Brandon
categories: [Security Projects]
tags: [container security]
---

Late last year at the end of November I decided to start working on some security engineering projects to learn more. I asked Claude AI for some project ideas and came across one for a security container pipeline project that scans docker containers for vulnerabilities. The idea being if this were some sort of production environment the docker builds would be scanned before being allowed into production.

[Link to Repo](https://github.com/BrandonF111/container-security-pipeline)

I followed the steps provided and got everything setup with Trivy as the initial scanner and ran some scans against a secure and vulnerable docker image. It was very interesting to see how many vulnerabilities can be found if something gets even moderately out of date. This reminded me how important it is to keep systems up to date, but also on the importance of security scanning systems before they go into production.

Also I learned about unfixed vulnerabilities and the thought process behind how to handle them. Since they would be an issue they can't be left as unfixed. But solutions such as an allowance with a date to check back in, or putting the system behind some sort firewall or restricted access, also downgrading to a version that may not have the vulnerability. I definitely looked into it and learned a lot in that regard, which was fun.

After working with that for a little while I decided to add another scanner based on what Claude suggested. I ended up adding Grype. I setup the GitHub actions to run off a workflow where I would input the directory name and the name of the container for it to scan. I will say I do like the output of Grype since without the grid table it's easier to read, but it comes in a risk first sorting so it took time to figure that out. 

The interesting part for me came when I decided to randomly check both scanners on the same image. When I did that I compared the two and saw different CVE's being listed. Some CVE's showed up only on Grype, and others only on Trivy. I asked about this and learned that it is due to what vulnerability database each scanner grabs their information from and in what order. This means that while one scanner may say a CVE is low, the other may say it is high due to where it is getting the information and how it is organizing it. This was neat to me because I didn't really think about this before, I just assumed that all scanners would put out the same information. Put after looking into it I learned that it can be good to have multiple scanners depending on what you are doing. 

After this I decided to make a [multi-scanner](https://github.com/BrandonF111/container-security-pipeline/blob/main/.github/workflows/multi-scan.yml) within my repo. I added Grype and Trivy and had the output compare the two and show the differences. It was interesting to see all the different vulnerabilities they detected and which ones were similar.

This entire project was actually fun to do and I definitely learned a lot. It was interesting seeing how many vulnerabilities can show on older systems, the need to figure out how to work around unfixed vulnerabilities, and how these two scanners detect differently and how you can use that to an advantage. I need to go back through it and take some more notes, since the learning never stops. But I am going to start working on another project to learn something else.

Brandon

> Written with [StackEdit](https://stackedit.io/).
