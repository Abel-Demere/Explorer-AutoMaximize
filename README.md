# Restore Explorer Windows

An AutoHotkey script that ensures Windows File Explorer maximizes when opening a folder while minimized. By default, Windows does not restore Explorer in this case, leaving it minimized.

## Compatibility
- Tested on **Windows 11 Dev Build**: **Version 23H2, OS Build 22635.4515**  
- **Might not work on previous Windows versions or the main 24H4 release** since the *"Open desktop folders and external folder links in new tab"* feature isn't added yet.  

## Features
- Automatically maximizes File Explorer when opening folders while minimized  
- Lightweight and efficient  
- Available as an AHK script or compiled EXE  

## Installation
### **1. Run the EXE (No installation required)**
- Download **`Restore Explorer Window.exe`** from [Releases](https://github.com/Abel-Demere/Restore-Explorer-Windows/tree/main/releases)  
- Double-click to run in the background  

### **2. Run the AHK script (Requires AutoHotkey)**
- Install [AutoHotkey](https://www.autohotkey.com/)  
- Download **`Restore Explorer Window.ahk`** from [Source](https://github.com/Abel-Demere/Restore-Explorer-Windows/tree/main/src)  
- Double-click to run  

## How It Works
This script detects when a new folder is opened and checks if File Explorer is minimized. If it is, the script restores the window.  

## Download
You can download the latest compiled EXE from the [Releases page](https://github.com/Abel-Demere/Restore-Explorer-Windows/tree/main/releases).  

## License
This project is licensed under the [MIT License](LICENSE).  
