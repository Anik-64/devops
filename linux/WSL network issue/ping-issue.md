# Fixing WSL Networking Issue by Switching Version

This guide explains how to resolve network connectivity problems (e.g., `ping` not working) in WSL by switching the Ubuntu distribution from **WSL 2** to **WSL 1**.

## Prerequisites
- Windows 10/11 with **WSL installed**
- At least one WSL distribution (e.g., Ubuntu)

## Steps

### 1. List Installed WSL Distributions

```powershell
wsl --list
```
Expected output (For me)  
`Windows Subsystem for Linux Distributions:`  
`Ubuntu (Default)`  
`docker-desktop`

### 2. Check WSL Versions of Distributions

```powershell
wsl --list --verbose
```
Example output  
`NAME STATE VERSION`    
`* Ubuntu Running 2`    
`docker-desktop Stopped 2`    

Here, Ubuntu is running with WSL 2.

### 3. Convert Ubuntu from WSL 2 to WSL 1
```powershell
wsl --set-version Ubuntu 1
```
### Result

After switching to WSL 1, network tools like ping should work correctly.
