---
title: Homelab setup
date: 07-08-2026
description: Description of my homelab setup
Author: Brandon
tags: [homelab]
---

# Homelab Setup
Since I'm working on documenting things better I wanted to post up some info about my home lab and how it is all setup. This has been a fun project and I definitely want to add to it when I can.

## Main System
The system everything runs off is an HP Z440. It has 64GB of RAM, and I swapped out the E5-1650 it came with for a E5-2690v4. From there I installed proxmox directly onto the system.

## VMs
A list of the VMs on my proxmox system.

Standalone Systems
 1. Ubuntu - This was my first VM I had with Wazuh installed. From here I watched my Windows system
 1. Kali box - Used to run attacks against my Windows client.
 1. OpenClaw (Ubuntu) - Built to run OpenClaw which I linked to my discord server in order to use.
 
Domain Systems
 1. Windows Server (AD DS) - My AD Server
 1. Windows Client 1 - Used initially to run attacks against from my Kali box in order to pick up the detections with Wazuh. This was later added to my AD domain.
 1. Windows Client 2 - This one was built to have another system connected to my domain so that could test out installing different apps under different users.
 1. Ubuntu - This one was created in order to add a Linux machine to my domain.
 1. Ubuntu Server (Ansible) - This system was created to run ansible as a control system in order to run playbooks on my two Windows clients, and my Ubuntu client.
![Homelab Topology](/assets/img/homelab_topology.png)
