# PS_Scripts # Detect local screen resolution

1. Run PS1 Scritp and it will autmatically open MSTSC.exe
2. Choose size for resolution
#. Copy script below and create a PS1 script in Windows Powershell ISE or paste in Notepad and save as PS1 file extension.


# Detect local screen resolution
Add-Type -AssemblyName System.Windows.Forms

$Screen = [System.Windows.Forms.Screen]::PrimaryScreen.Bounds

$NativeWidth  = $Screen.Width
$NativeHeight = $Screen.Height

# Customize scaling percentage
# Example:
# 1.0 = full resolution
# 0.75 = 75%
# 0.5 = half size

$Scale = 0.75

$RDPWidth  = [int]($NativeWidth * $Scale)
$RDPHeight = [int]($NativeHeight * $Scale)

# Remote computer name
$Computer = "SERVER01"

# Create temporary RDP file
$RDPFile = "$env:TEMP\custom_rdp.rdp"

@"
screen mode id:i:2
use multimon:i:0
desktopwidth:i:$RDPWidth
desktopheight:i:$RDPHeight
session bpp:i:32
compression:i:1
keyboardhook:i:2
audiocapturemode:i:0
videoplaybackmode:i:1
connection type:i:7
networkautodetect:i:1
bandwidthautodetect:i:1
displayconnectionbar:i:1
disable wallpaper:i:0
allow font smoothing:i:1
allow desktop composition:i:1
full address:s:$Computer
prompt for credentials:i:1
administrative session:i:0
"@ | Set-Content $RDPFile

# Launch RDP session
Start-Process "mstsc.exe" -ArgumentList $RDPFile
