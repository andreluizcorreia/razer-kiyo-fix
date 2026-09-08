# Razer Kiyo & Kiyo Pro UVC Firmware & Driver Fix

This repository provides a reliable software workaround and PowerShell automation script to fix persistent UVC firmware freezes, "camera in use by another application" errors, and broken device enumeration loops for **Razer Kiyo** and **Razer Kiyo Pro** webcams on Windows.

## 🛠 Symptoms Addressed
* Webcam completely disappearing from Device Manager or showing a yellow exclamation mark status.
* Persistent errors stating the camera is in use by another application even when all apps are closed.
* Inability to update firmware or initialize the UVC stream due to deadlocked Windows Media Foundation handles.

---

## 🚀 Quick Fix Instructions

1. Open **PowerShell** or **Windows Terminal** as **Administrator**.
2. Run the automation script below to clear background process locks and force-flush corrupted PnP device registry entries:

```powershell
# 1. Terminate competing capture processes and restart the Windows Media FrameServer service
Stop-Process -Name "obs64", "Discord", "Skype", "Razer Synapse" -Force -ErrorAction SilentlyContinue; Restart-Service -Name FrameServer -Force

# 2. Force-remove corrupted ghost instances and registry locks for the Razer Kiyo
Get-PnpDevice -FriendlyName "*Razer Kiyo*" | ForEach-Object {
    $id =$_.InstanceId -replace '\\','\'
    cmd /c "pnputil /remove-device ""$id"" /force"
}
