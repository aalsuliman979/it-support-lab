Virtual Machine Setup

## Goal
Create a virtual server that will act as the domain controller (DC01) for a small simulated company network.

## What I did
1. Installed Oracle VirtualBox on my PC.
2. Downloaded the Windows Server 2022 evaluation ISO from Microsoft.
3. Created a VM named DC01 (4 GB RAM, 2 CPUs, 50 GB disk).
4. Installed Windows Server 2022 Standard (Desktop Experience).
5. Renamed the server to DC01.

## Why
- A VM lets me practice server administration without extra hardware.
- I chose Desktop Experience because it includes the graphical interface, which is easier to learn on.
- I disabled Unattended Installation so I could go through each install step myself.

## Issues I faced
Problem: Ctrl+Alt+Del was sent to my main PC instead of the VM.
Solution: Used Input > Keyboard > Insert Ctrl-Alt-Del from the VirtualBox menu.

## Screenshots
- 01-vm-settings.png
- 03-first-login.png
- 04-computer-name.pngs