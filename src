; Set a timer to check for Explorer windows every 100ms
SetTimer(() => CheckExplorerWindows(), 100)

; Global variable to store previously tracked windows, paths, and instance counts
global previousWindows := Map()

; Function to check for Explorer windows and restore them if necessary
CheckExplorerWindows() {
    ; Array to hold current Explorer windows
    currentWindows := []

    ; Get a list of File Explorer windows
    for hwnd in WinGetList("ahk_class CabinetWClass") {
        ; Check if the window still exists
        if WinExist("ahk_id " hwnd) {
            ; Get the active directory and process ID for the window
            dir := GetExplorerPath(hwnd)
            if (dir != "") {
                ; Get the process ID (PID) of the window
                pid := WinGetPID(hwnd)
                ; Add the window details to the currentWindows array
                currentWindows.Push({ hwnd: hwnd, path: dir, pid: pid })
            }
        }
    }

    ; Compare current windows with previously tracked windows
    for window in currentWindows {
        hwnd := window.hwnd
        path := window.path
        pid := window.pid

        if !previousWindows.Has(hwnd) {
            ; New instance detected
            previousWindows[hwnd] := { path: path, pid: pid, instanceCount: 1 }
            ; Restore the window
            RestoreWindow(hwnd)
        } else {
            ; Get the previously stored data for the window
            prevData := previousWindows[hwnd]

            if (prevData.path != path || prevData.pid != pid) {
                ; Path or PID has changed (new tab or folder opened)
                previousWindows[hwnd] := { path: path, pid: pid, instanceCount: 1 }
                ; Restore the window
                RestoreWindow(hwnd)
            } else {
                ; Same path and PID: Check for additional instances
                instanceCount := CountInstances(path)
                if (instanceCount > prevData.instanceCount) {
                    ; Update the instance count
                    previousWindows[hwnd].instanceCount := instanceCount
                    ; Restore the window
                    RestoreWindow(hwnd)
                }
            }
        }
    }

    ; Clean up closed windows
    for hwnd in previousWindows {
        if !WinExist("ahk_id " hwnd) {
            ; Remove the window from the map if it no longer exists
            previousWindows.Delete(hwnd)
        }
    }
}

; Function to restore and activate the window if minimized
RestoreWindow(hwnd) {
    ; Check if the window is minimized
    if (WinGetMinMax(hwnd) == -1) {
        ; Restore the window
        WinRestore(hwnd)
        ; Activate the window
        WinActivate(hwnd)
    }
}

; Function to retrieve the current folder path of the File Explorer window
GetExplorerPath(hwnd) {
    ; Iterate through the Shell.Application windows
    for window in ComObject("Shell.Application").Windows {
        try {
            ; Check if the window handle matches
            if (window.HWND == hwnd)
                ; Return the folder path
                return window.Document.Folder.Self.Path
        } catch {
            ; Continue to the next window if an error occurs
            continue
        }
    }
    ; Return an empty string if the path is not found
    return ""
}

; Function to count the number of instances of a specific folder path
CountInstances(path) {
    ; Initialize the instance count
    instanceCount := 0
    ; Iterate through the Shell.Application windows
    for window in ComObject("Shell.Application").Windows {
        try {
            ; Check if the folder path matches
            if (window.Document.Folder.Self.Path = path)
                ; Increment the instance count
                instanceCount++
        } catch {
            ; Continue to the next window if an error occurs
            continue
        }
    }
    ; Return the instance count
    return instanceCount
}

; Function to get the process ID (PID) of the window
WinGetPID(hwnd) {
    ; Initialize the PID variable
    pid := 0
    ; Use DllCall to get the process ID of the window
    DllCall("GetWindowThreadProcessId", "UInt", hwnd, "UIntP", pid)
    ; Return the PID
    return pid
}
