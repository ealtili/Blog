# **Windows Sandbox**
------------------------------

**Windows Sandbox is a new lightweight desktop environment tailored for safely running applications in isolation.**

Windows Sandbox is built based on Windows Container technology, which allows you to spin up an isolated, temporary, desktop environment where you can run untrusted software. The software you run and install in the Windows Sandbox does not affect the host. If you shut down the Windows Sandbox all changes and all software you installed in the Sandbox are deleted. This is similar  Windows Defender Application Guard already used to build a sandbox environment for Microsoft Edge.

Windows Sandbox has the following properties:

-   **Part of Windows** - everything required for this feature ships with Windows 10 Pro and Enterprise. No need to download a VHD!
-   **Pristine** - every time Windows Sandbox runs, it's as clean as a brand-new installation of Windows
-   **Disposable** - nothing persists on the device; everything is discarded after you close the application
-   **Secure** - uses hardware-based virtualization for kernel isolation, which relies on the Microsoft's hypervisor to run a separate kernel which isolates Windows Sandbox from the host
-   **Efficient** - uses integrated kernel scheduler, smart memory management, and virtual GPU

**Windows Sandbox Architecture**
-------------------------

Windows Sandbox builds on [Windows Containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/). 

**Dynamically generated Image** 
- Windows Sandbox leverages the Windows 10 installed on your computer.
- Windows Sandbox always present a clean environment, by using "dynamic base image": Most OS files are immutable and can be freely shared with Windows Sandbox. A small subset of operating system files are mutable and cannot be shared, so the sandbox base image contains original copies of them. A complete Windows image can be constructed from a combination of the sharable immutable files on the host and the original copies of the mutable files. 
- Windows Sandbox has a full Windows installation to boot from without needing to download or store an additional copy of Windows. 
- When Windows Sandbox is not installed, dynamic base image is a compressed package which is only 30MB. When installed the dynamic base package it occupies about 500MB disk space.

![Dynamic Image.PNG](https://gxcuf89792.i.lithium.com/t5/image/serverpage/image-id/62787i9053915B9D0FABFC/image-size/large?v=1.0&px=999 "Dynamic Image.PNG")

**Smart Memory Management and Share** 
- If the host is under memory pressure, it can reclaim memory from the container much like it would with a process. 
![Memory Management](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-sandbox/images/2-dynamic-working.png)
- Windows sandbox to use the same physical memory pages as the host for operating system binaries via a technology refered to as "direct map". 
![Direct Map.PNG](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-sandbox/images/3-memory-sharing.png "Direct Map.PNG")
  
**Integrated kernel scheduler** 
– More responsiveness by better-prioritizing processes on the host and in the sandbox.
- Windows Sandbox use a new technology called "integrated scheduler" which allows the host to decide when the sandbox runs.
- Windows Sandbox employ a unique scheduling policy that allows the virtual processors of the sandbox to be scheduled in the same way as threads would be scheduled for a process. High-priority tasks on the host can preempt less important work in the sandbox. The benefit of using the integrated scheduler is that the host manages Windows Sandbox as a process rather than a virtual machine which results in a much more responsive host, similar to [Linux KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine).
- The whole goal here is to treat the Sandbox like an app but with the security guarantees of a Virtual Machine.

![Integrated kernel scheduler](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-sandbox/images/4-integrated-kernal.png)

**Snapshot and clone** 
Improves start time of the sandbox
**Graphics virtualization** 
Benefit from hardware accelerated rendering. A system with a compatible GPU and graphics drivers (WDDM 2.5 or newer) is required. Incompatible systems will render apps in Windows Sandbox with Microsoft's CPU-based rendering technology, Windows Advanced Rasterization Platform (WARP).
![WDDM GPU virtualization](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-sandbox/images/5-wddm-gpu-virtualization.png)
**Battery pass-through** 
Aware of the host’s battery state, which allows it to optimize power consumption. Similar to the Windows 10 Hyper-V Battery pass-through.

**Prerequisites for Windows Sandbox**
----

-   Windows 10 Pro or Enterprise build 18305 or later
-   AMD64 architecture
-   Virtualization capabilities enabled in BIOS
-   At least 4GB of RAM (8GB recommended)
-   At least 1 GB of free disk space (SSD recommended)
-   At least 2 CPU cores (4 cores with hyperthreading recommended)

Quick start
-----------

1.  Ensure that your machine is using Windows 10 Pro or Enterprise, build version 18305 or later.
2.  Enable virtualization:
    -   If you are using a physical machine, ensure virtualization capabilities are enabled in the BIOS.
    -   If you are using a virtual machine, enable nested virtualization with this PowerShell cmdlet:
``` Powershell
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true 
```
3.  Open Windows Features, and then select Windows Sandbox. Select **OK** to install Windows Sandbox. You might be asked to restart the computer.
  ![Optional Windows Features dlg.png](https://gxcuf89792.i.lithium.com/t5/image/serverpage/image-id/62785i7A89663EBE5E0521/image-size/medium?v=1.0&px=400 "Optional Windows Features dlg.png")

You can also run the following PowerShell command:
``` Powershell
Enable-WindowsOptionalFeature -FeatureName "Containers-DisposableClientVM" -Online
```
4.  Using the **Start** menu, find Windows Sandbox, run it and allow the elevation
[![Windows 10 Windows Sandbox Start](https://www.thomasmaurer.ch/wp-content/uploads/2019/05/Windows-10-Windows-Sandbox-Start-768x491.jpg)](https://www.thomasmaurer.ch/wp-content/uploads/2019/05/Windows-10-Windows-Sandbox-Start.jpg)

or just run WindowsSandbox.exe cmd C:\Users\\%username%\WindowsSandbox.exe
5.  Copy an executable file from the host
6.  Paste the executable file in the window of Windows Sandbox (on the Windows desktop)
7.  Run the executable in the Windows Sandbox; if it is an installer go ahead and install it
8.  Run the application and use it as you normally do
9. When you're done experimenting, you can simply close the Windows Sandbox application. All sandbox content will be discarded and permanently deleted
10. Confirm that the host does not have any of the modifications that you made in Windows Sandbox.

[![Windows Sandbox](https://gxcuf89792.i.lithium.com/t5/image/serverpage/image-id/62786i559AE3EDA7C968A4?v=1.0)](https://www.microsoft.com/en-us/videoplayer/embed/RE4rFAo)

Windows Sandbox respects the host diagnostic data settings. All other privacy settings are set to their default values.

## Windows Sandbox - Configuration 
-------------------------------------------------------------------

Overview
--------

Sandbox configuration files are formatted as XML, and are associated with Windows Sandbox via the .wsb file extension. A configuration file allows the user to control the following aspects of Windows Sandbox:

1. vGPU (virtualized GPU): Enable or disable the virtualized GPU. If vGPU is disabled, the sandbox will use Windows Advanced Rasterization Platform (WARP).
2. Networking: Enable or disable network access within the sandbox.
3. Mapped folders: Share folders from the host with read or write permissions. Note that exposing host directories may allow malicious software to affect the system or steal data.
4. Logon command: A command that's executed when Windows Sandbox starts.
5. Audio input: Shares the host's microphone input into the sandbox.
6. Video input: Shares the host's webcam input into the sandbox.
7. Protected client: Places increased security settings on the RDP session to the sandbox.
8. Printer redirection: Shares printers from the host into the sandbox.
9. Clipboard redirection: Shares the host clipboard with the sandbox so that text and files can be pasted back and forth.
10. Memory in MB: The amount of memory, in megabytes, to assign to the sandbox.

![SandboxConfigFile.png](https://gxcuf89792.i.lithium.com/t5/image/serverpage/image-id/83963iDD2AAF0FC1A2A860/image-size/medium?v=1.0&px=400 "SandboxConfigFile.png")

Configuration files can be used to granularly control Windows Sandbox for enhanced isolation.

Double click a config file to open it in Windows Sandbox, or invoke it via the command line as shown:

    C:\Temp> MyConfigFile.wsb

Here is a quick overview of the different settings you can use in the config files.

| NAME | SETTING | SUBSETTING | VALUES |
| --- | --- | --- | --- |
| Virtual GPU | vGPU |  | Disable/Enable -  (Default - vGPU Disabled) 
| Networking | Networking |  | Disable - Disables Networking (Default - Networking enabled) 
| Shared Folder | MappedFolder | HostFolder | Path to the host folder ( folder must already exist on the host, or the container will fail to start.) 
|  |  | SandboxFolder | Path to the Sandbox folder (If no sandbox folder is specified, the folder will be mapped to the container desktop.) 
|  |  | ReadOnly | True/False (Default False)
| Startup Script | LogonCommand | Command | Command which gets executed 
| Audio Input | AudioInput||Enable/Disable (Default Enabled)
| Video Input | VideoInput||Enable/Disable (Default Disabled)
| Protected Client | ProtectedClient||Enable/Disable (Default Disabled)
| Printer Redirection | PrinterRedirection||Enable/Disable (Default Disabled)
|Clipboard Redirection| ClipboardRedirection|| Disable - Disable Clipboard (Default Enabled)
|Memory In MB|MemoryInMB||If the memory value specified is insufficient to boot a sandbox, it will be automatically increased to the required minimum amount.

Windows Sandbox Config Files, Values and Limits
---------------------------


**vGPU**: Enables or disables GPU sharing.

    <vGPU>value</vGPU>

*Supported values*:

Enable: Enables vGPU support in the sandbox.
Disable: Disables vGPU support in the sandbox. If this value is set, the sandbox will use software rendering, which may be slower than virtualized GPU.
Default vGPU support Disabled. 
 
Enabling virtualized GPU can potentially increase the attack surface of the sandbox.

**Networking**: Enables or disables networking in the sandbox. You can disable network access to decrease the attack surface exposed by the sandbox.

    <Networking>value</Networking>

*Supported values*:

Disable: Disables networking in the sandbox.
Default: This is the default value for networking support. This value enables networking by creating a virtual switch on the host and connects the sandbox to it via a virtual NIC.
 
Enabling networking can expose untrusted applications to the internal network.

**Mapped folders**: An array of folders, each representing a location on the host machine that will be shared into the sandbox at the specified path. At this time, relative paths are not supported. If no path is specified, the folder will be mapped to the container user's desktop.

XML
```XML
<MappedFolders>
  <MappedFolder> 
    <HostFolder>absolute path to the host folder</HostFolder> 
    <SandboxFolder>absolute path to the sandbox folder</SandboxFolder> 
    <ReadOnly>value</ReadOnly> 
  </MappedFolder>
  <MappedFolder>  
    ...
  </MappedFolder>
</MappedFolders>
```
***HostFolder***: Specifies the folder on the host machine to share into the sandbox. Note that the folder must already exist on the host, or the container will fail to start.

***SandboxFolder***: Specifies the destination in the sandbox to map the folder to. If the folder doesn't exist, it will be created. If no sandbox folder is specified, the folder will be mapped to the container desktop.

***ReadOnly***: If true, enforces read-only access to the shared folder from within the container. Supported values: true/false. Defaults to false.

Files and folders mapped in from the host can be compromised by apps in the sandbox or potentially affect the host.

**Logon command**: Specifies a single command that will be invoked automatically after the sandbox logs on. Apps in the sandbox are run under the container user account.

XML
``` XML
<LogonCommand>
  <Command>command to be invoked</Command>
</LogonCommand>
```
***Command***: A path to an executable or script inside the container that will be executed after login.

Although very simple commands will work (such as launching an executable or script), more complicated scenarios involving multiple steps should be placed into a script file. This script file may be mapped into the container via a shared folder, and then executed via the LogonCommand directive.

**Audio input**: Enables or disables audio input to the sandbox.

    <AudioInput>value</AudioInput>

***Supported values***:

Enable: Enables audio input in the sandbox. If this value is set, the sandbox will be able to receive audio input from the user. Applications that use a microphone may require this capability.
Disable: Disables audio input in the sandbox. If this value is set, the sandbox can't receive audio input from the user. Applications that use a microphone may not function properly with this setting.
Default: This is the default value for audio input support. Currently this means audio input is enabled.

There may be security implications of exposing host audio input to the container.

**Video input**: Enables or disables video input to the sandbox.

    <VideoInput>value</VideoInput>

***Supported values***:

Enable: Enables video input in the sandbox.
Disable: Disables video input in the sandbox. Applications that use video input may not function properly in the sandbox.
Default: This is the default value for video input support. Currently this means video input is disabled. Applications that use video input may not function properly in the sandbox.

There may be security implications of exposing host video input to the container.

**Protected client**: Applies additional security settings to the sandbox Remote Desktop client, decreasing its attack surface.

    <ProtectedClient>value</ProtectedClient>

***Supported values***:

Enable: Runs Windows sandbox in Protected Client mode. If this value is set, the sandbox runs with extra security mitigations enabled.
Disable: Runs the sandbox in standard mode without extra security mitigations.
Default: This is the default value for Protected Client mode. Currently, this means the sandbox doesn't run in Protected Client mode.

This setting may restrict the user's ability to copy/paste files in and out of the sandbox.

**Printer redirection**: Enables or disables printer sharing from the host into the sandbox.

    <PrinterRedirection>value</PrinterRedirection>

Supported values:

Enable: Enables sharing of host printers into the sandbox.
Disable: Disables printer redirection in the sandbox. If this value is set, the sandbox can't view printers from the host.
Default: This is the default value for printer redirection support. Currently this means printer redirection is disabled.

**Clipboard redirection**: Enables or disables sharing of the host clipboard with the sandbox.

    <ClipboardRedirection>value</ClipboardRedirection>

***Supported values***:

Disable: Disables clipboard redirection in the sandbox. If this value is set, copy/paste in and out of the sandbox will be restricted.
Default: This is the default value for clipboard redirection. Currently copy/paste between the host and sandbox are permitted under Default.

**Memory in MB***: Specifies the amount of memory that the sandbox can use in megabytes (MB).

    <MemoryInMB>value</MemoryInMB>

If the memory value specified is insufficient to boot a sandbox, it will be automatically increased to the required minimum amount.

## Examples
-----
### Example 1:

The following config file can be used to easily test downloaded files inside of the sandbox. To achieve this, the script disables networking and vGPU, and restricts the shared downloads folder to read-only access in the container. For convenience, the logon command opens the downloads folder inside of the container when it is started.

**Downloads.wsb**
``` XML
  <Configuration>
  <VGpu>Disable</VGpu>
  <Networking>Disable</Networking>
  <MappedFolders>
    <MappedFolder>
      <HostFolder>C:\Users\Public\Downloads</HostFolder>
      <SandboxFolder>C:\Users\WDAGUtilityAccount\Downloads</SandboxFolder>
      <ReadOnly>true</ReadOnly>
    </MappedFolder>
  </MappedFolders>
  <LogonCommand>
    <Command>explorer.exe C:\users\WDAGUtilityAccount\Downloads</Command>
  </LogonCommand>
</Configuration>
```
### Example 2

The following config file installs Visual Studio Code in the container, which requires a slightly more complicated LogonCommand setup.

Two folders are mapped into the container; the first (SandboxScripts) contains VSCodeInstall.cmd, which will install and run VSCode. The second folder (CodingProjects) is assumed to contain project files that the developer wants to modify using VSCode.

With the VSCode installer script already mapped into the container, the LogonCommand can reference it.

**VSCodeInstall.cmd**
``` CMD
REM Download VSCode
curl -L "https://update.code.visualstudio.com/latest/win32-x64-user/stable" --output C:\users\WDAGUtilityAccount\Desktop\vscode.exe

REM Install and run VSCode
C:\users\WDAGUtilityAccount\Desktop\vscode.exe /verysilent /suppressmsgboxes
```
**VSCode.wsb**
``` XML
<Configuration>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\SandboxScripts</HostFolder>
     <ReadOnly>true</ReadOnly>
   </MappedFolder>
   <MappedFolder>
     <HostFolder>C:\CodingProjects</HostFolder>
     <ReadOnly>false</ReadOnly>
   </MappedFolder>
</MappedFolders>
<LogonCommand>
   <Command>C:\users\wdagutilityaccount\desktop\SandboxScripts\VSCodeInstall.cmd</Command>
</LogonCommand>
</Configuration>
```

### Example 3

Here is one which mounts local download folder read-only from host, into the sandbox.
``` XML
<Configuration>
<VGpu>Default</VGpu>
<Networking>Default</Networking>
<MappedFolders>
   <MappedFolder>
     <HostFolder>C:\Users\admin\Downloads</HostFolder>
     <ReadOnly>true</ReadOnly>
   </MappedFolder>
</MappedFolders>
<LogonCommand>
   <Command>explorer.exe C:\users\WDAGUtilityAccount\Desktop\Downloads</Command>
</LogonCommand>
</Configuration>
```
This means download folder (C:\Users\admin\Downloads) will be mounted in the desktop folder (C:\Users\WDAGUtilityAccount\Desktop\Downloads) of the sandbox. With the command "explorer.exe C:\users\WDAGUtilityAccount\Desktop\Downloads" it will directly open up the download folder in an explorer window.

### Example 4
This example installs the Microsoft Edge Chromium version inside the Windows Sandbox. I stored the MicrosoftEdgeChromiumSetup.exe in my download folder. In the config file, I mount the download folder and run the MicrosoftEdgeChromiumSetup.exe.
``` XML
<Configuration>
<MappedFolders>
<MappedFolder>
<HostFolder>C:\Users\admin\Downloads</HostFolder>
<ReadOnly>true</ReadOnly>
</MappedFolder>
</MappedFolders>
<LogonCommand>
<Command>C:\Users\WDAGUtilityAccount\Desktop\Downloads\MicrosoftEdgeChromiumSetup.exe</Command>
</LogonCommand>
</Configuration>
```
### Example 5
If you want to work with Sysinternals, you can also just easily mount the Sysinternals SMB share using the following config file.
``` XML
<Configuration>
<LogonCommand>
<Command>net use S: \\live.sysinternals.com\tools</Command>
</LogonCommand>
</Configuration>
```
