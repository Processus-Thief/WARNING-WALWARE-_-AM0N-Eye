!!! WARNING !!!

This fork is for educational purpose only

This repository has been publicly accessible but it seems to contain some malware, so USE WITH CAUTION !


----------------------------

# AM0N-Eye
The most focused point for the development is the collection of projects for the Cobaltsetrike and making it an essential feature without the need for me to add it every time.
AM0N-Eye is the most advanced Red Team & Adversary Simulation Software in the current C2 Market. It can not only emulate different stages of an attacker killchain, but also provide a systematic timeline and graph for each of the attacks executed to help the Security Operations Team validate the attacks and improve the internal defensive mechanisms. AM0N-Eye comes prebuilt with several opsOpec features which can ease a Red Team’s task to focus more on the analytical part of an engagement instead of focusing or depending on Open source tools for post-exploitation. AM0N-Eye is a post-exploitation C2 in the end and however does not provide exploit generation features like metasploit or vulnerability scanning features like Nessus, Acunetix or BurpSuite. AM0N-Eye is a project based on a combination of different ideas and projects used by the threat actor where we observe a set of techniques to evasion EDR and AV while allowing the operator to continue using the tools The C2 standard is specifically designed to succeed in mature environments. Note here that they each have something that sets them apart, such as c2-backed shadow dedicated to Liunx and MacOS, brute ratel and its ability to evasion defensive machines,and also Sliver that support C2 over Mutual TLS (mTLS), WireGuard, HTTP(S), and DNS and are dynamically compiled with per-binary asymmetric encryption keys. Of course, I do not forget Cobaltsetrike, which is the most exploited here because it is the basis of this environment, especially the project that was uploading in the script console and it was the best environment to modify it and add all these features. So what if we combined all these features in one environment that works  With the mechanism together, with basic ttps added in any APT attack, and here I will know some TTPs of AM0N-Eye, but not all.

1. Linux, MacOS and windows c2 server
2. Fake Alert techniques
3. AV/EDR evasion techniques
4. shellcode Generator & obfuscatior
5. Persistence techniques
6. New BOF
7. AV/EDR Recon
8. PayloadGenerator Undetected by antivirus programs
9. custom malwares
10. New c2 profiles

![Screenshot from 2023-03-10 11-53-32](https://user-images.githubusercontent.com/121706460/226493992-1b6194b7-13a3-4ac5-bb3c-d473bbf0dd31.png)

<install>

chmod +x install.sh

chmod +x teamserver.AppImage

chmod +x st.AppImage
	
chmod +x start.sh   👈️ You can modify the start.sh file and put your ip in the run command to make the boot process easier
	
sudo ./install.sh

<start>

sudo ./teamserver.AppImage <yourip> password & ./st.AppImage
__________________________________________________________________________________________________________________________________________________________
##PayloadGenerator

Generates every type of Stageless/Staged Payload based off a HTTP/HTTPS Listener Undetected by antivirus programs
    
Creates /opt/amon-eye/Staged_Payloads, /opt/amon-eye/Stageless_Payloads
    
#Linux & MacOS C2 Server

A security framework for enterprises and Red Team personnel, supports AM0N-Eye penetration testing of other platforms (Linux / MacOS / ...), supports custom modules, and includes some commonly used penetration modules.

Lateral movement

    Generate beacon of Linux-bind / MacOS-bind type
    The target in the intranet runs ./MacOS-bind.beacon <port> to start the service
    Run connect <targetIP>:<port> in the session
    
Examples

The script interpreter such as bash / python / ruby / perl / php in the host can be called directly in the session to execute the script passed into the memory. There is no information in the process, all running content is transferred from the memory to the interpreter

    1.python c:\getsysteminfo.py
    2.python import base64;print base64.b64encode('whoami'); print 'a'*40
    3.php

Don't forget to Check C2 profiles in /AM0N-Eye/C2-Profiles/ to bypass network filters
To use a custom profile  you must start a AM0N-Eye team server and specify your profile at that tim 
Example ./teamserver [external IP] [password] [/path/to/my.profile] .

![Screenshot from 2023-03-09 13-47-25](https://user-images.githubusercontent.com/121706460/226558264-db460f06-92f1-445e-b428-80a13a69f487.png)

	
# Fake Alert update

to send toast notifications on behalf on an (installed) application or the computer itself. The user will be asked to supply credentials once they click on the notification toast. The second one is a AM0N-Eye module to launch the phishing attack on connected beacons and you can learn the types of victim's defense mechanisms and exploit this to issue an update alert or to take action

![Screenshot from 2023-02-21 02-42-37](https://user-images.githubusercontent.com/121706460/226552401-6666bc29-2b9b-4248-9056-faafe28af324.png)


#AV/EDR evasion

(AV/EDR evasion) is a payload creation framework for side loading (not injecting) into a legitimate Windows process (bypassing Application Whitelisting controls). Once the DLL loader is loaded into memory, it utilizes a technique to flush an EDR’s hook out of the system DLLs running in the process's memory. This works because we know the EDR’s hooks are placed when a process is spawned. (AV/EDR evasion) can target these DLLs and manipulate them in memory by using the API function VirtualProtect, which changes a section of a process’ memory permissions to a different value, specifically from Execute–Read to Read-Write-Execute.

When executed, (AV/EDR evasion) will copy the bytes of the system DLLs stored on disk in C:\Windows\System32\. These DLLs are stored on disk “clean” of EDR hooks because they are used by the system to load an unaltered copy into a new process when it’s spawned. Since EDR’s only hook these processes in memory, they remain unaltered. (AV/EDR evasion) does not copy the entire DLL file, instead only focuses on the .text section of the DLLs. This section of a DLL contains the executable assembly, and by doing this (AV/EDR evasion) helps reduce the likelihood of detection as re-reading entire files can cause an EDR to detect that there is a modification to a system resource. The data is then copied into the right region of memory by using each function’s offset. Each function has an offset which denotes the exact number of bytes from the base address where they reside, providing the function’s location on the stack.

To do this, (AV/EDR evasion) changes the permissions of the .text region of memory using VirtualProtect. Even though this is a system DLL, since it has been loaded into our process (that we control), we can change the memory permissions without requiring elevated privileges.

Once these the hooks are removed, (AV/EDR evasion) then utilizes custom System Calls to load and run shellcode in memory. (AV/EDR evasion) does this even after the EDR hooks are removed to help avoid detection by non-userland, hook-based telemetry gathering tools such as Event Tracing for Windows (ETW) or other event logging mechanisms. These custom system calls are also used to perform the VirtualProtect call to remove the hooks placed by EDRs, described above, to avoid detection by any EDR’s anti-tamper controls. This is done by calling a custom version of the VirtualProtect syscall, NtProtectVirtualMemory. (AV/EDR evasion) utilizes Golang to generate these loaders and then assembly for these custom syscall functions.

(AV/EDR evasion) loads the shellcode into memory by first decrypting the shellcode, which is encrypted by default using AES encryption with a decryption and initialization vector key. Once decrypted and loaded, the shellcode is then executed. Depending on the loader options specified (AV/EDR evasion) will set up different export functions for the DLL. The loaded DLL also does not contain the standard DLLmain function which all DLLs typically need to operate. The DLL will still execute without any issue because the process we load into will look for those export functions and not worry about DLLMain being there.

 ___________________________________________________________________
|                Various Out-Of-Box Evasion Capabilities            |
|-------------------------------------------------------------------|
|Evasion Capabilities 	x64 Support 	x86 |Support |x86 on Wow64  |
|Indirect System Calls 	Yes 	Yes 	Yes |   yes  |     yes      |
|Hide Shellcode Sections in Memory 	    Yes |	Yes  |	   Yes      |
|Multiple Sleeping Masking Techniques 	Yes |	yes  |	   yes      |
|Unhook EDR Userland Hooks and Dlls 	Yes |	yes  |	   yes      |
|LoadLibrary Proxy for ETW Evasion      Yes |	yes  |	   yes      |
|Thread Stack Encryption 	    Yes 	Yes |	Yes  |     yes      |
|Badger Heap Encryption      	Yes 	Yes |	Yes  |     yes      |
|Masquerade Thread Stack Frame 	Yes 	Yes |	Yes  |     yes      | 
|Hardware Breakpoint for AMSI/ETW Evasion   |	Yes  |	   Yes 	    |
|Reuse Virtual Memory For ETW Evasion 	Yes |	Yes  |	   Yes      |
|Reuse Existing Libraries from PEB 	    Yes |__ Yes  |	   Yes      |
|Secure Free Badger Heap for Volatility Evasion| Yes |	   Yes      |
|______________________________________________|_____|______________|

(AV/EDR evasion) contains the ability to do process injection attacks. To avoid any hooking or detection in either the loader process or the injected process itself, (AV/EDR evasion) first unhooks the loader process as it would normally, to ensure there are no hooks in the process. Once completed, the loader will then spawn the process specified in the creation command. Once spawned, the loader will then create a handle to the process to retrieve a list of loaded DLLs. Once it finds DLLs, it will enumerate the base address of each DLL in the remote process. Using the function WriteProcessMemory the loader will then write the bytes of the system DLLs stored on disk (since they are “clean” of EDR hooks) without the need to change the memory permissions first. (AV/EDR evasion) uses WriteProcessMemory because this function contains a feature primarily used in debugging where even if a section of memory is read-only, if everything is correct in the call to Write­Process­Memory, it will temporarily change the permission to read-write, update the memory section and then restore the original permissions. Once this is done, the loader can inject shellcode into the spawned process with no issue, as there are no EDR hooks in either process.
	
	
![Screenshot from 2023-03-21 04-48-45](https://user-images.githubusercontent.com/121706460/226556701-11379ed8-66de-4303-9daf-aca85f78af85.png)

	
#shellcode obfuscatior
 
Generates beacon stageless shellcode with exposed exit method, additional formatting, encryption, encoding, compression, multiline output, etc
shellcode transforms are generally performed in descending menu order
Requirements:
The optional AES encryption option uses a python script in the /assets folder
Depends on the pycryptodome package to be installed to perform the AES encryption

Install pycryptodome with pip depending on your python environment:

python -m pip install pycryptodome
python3 -m pip install pycryptodome
py -3 -m pip install pycryptodome
py -2 -m pip install pycryptodome

Listener:
Select a valid listener with the "..." button. Shellcode will be generated form this listener selection

Delivery:
Stageless (Staged not supported for the shellcode generator)

Exit Method:
process - exits the entire process that beacon is present in when the beacon is closed
thread - exits only the thread in which beacon is running when the beacon is closed

Local Pointers Checkbox:
May use if you are going to execute the shellcode from an existing Beacon
Generates a Beacon shellcode payload that inherits key function pointers from a same-arch parent Beacon

Existing Session:
Only used if the Local Pointers checkbox is checked
The parent Beacon session where the shellcode will pull session metadata
Shellcode should be run from within this Beacon session

x86 Checkbox:
Check to generate x86 shellcode, x64 is generated by default

Or Use Shellcode File:
Use an externally generated raw shellcode file in lieu of generating Beacon shellcode
This allows you to use previously exported shellcode files or output from other tools (Donut, msfvenom, etc)

Formatting:

raw - raw binary shellcode output, no formatting applied
hex - hex formatted shellcode output
0x90,0x90,0x90 - shellcode formatted into a C# style byte array (example format, does not prepend nulls)
0x90uy;0x90uy;0x90uy - shellcode formatted into a F# style byte array (example format, does not prepend nulls)
\x90\x90\x90 - shellcode formatted into a C\C++ style byte array (example format, does not prepend nulls)
b64 - option to base64 encode the shellcode early in the generation process (before any encryption)

XOR Encrypt Shellcode Checkbox:
Check to XOR encrypt the shellcode (only one encryption type can be selected at a time)

XOR Key(s):
Randomly generated and editable XOR key character(s) to use for encryption
Multiple characters will result in multiple rounds of XOR encryption (i.e. ABCD)

AES Encrypt Shellcode Checkbox:
Check to AES encrypt the shellcode (only one encryption type can be selected at a time)
Uses a python script to perform AES Block Cipher AES-CBC encryption
Shellcode is padded with \0 values to reach block size requirements
A randomly generated IV is prepended to the encrypted shellcode data

AES Key:
Randomly generated and editable AES key to use for encryption
32byte key is generated and preferred for 256bit encryption strength
Encryption key byte lengths accepted are 16, 24, and 32

Encoding/Compression:
none - No additional encoding or compression is done to the shellcode
b64 - base64 encode the shellcode
gzip then b64 - gzip compress then base64 the shellcode
gzip - gzip compress the shellcode
b64 then gzip - base64 then gzip compress the shellcode
b64 then 7xgzip - base64 then gzip compress the shellcode 7 times
	
![Screenshot from 2023-03-21 04-46-30](https://user-images.githubusercontent.com/121706460/226556899-c1253b00-8e08-469c-9a46-f1012b1f2795.png)


# Persistence threat _Menu
		
![Screenshot from Screencast 2023-03-22 08-14-28 mp4](https://user-images.githubusercontent.com/121706460/226905003-ff4a8f85-de5a-4ad1-840f-0a3f411db32c.png)
	
* (UserSchtasksPersist)

Schtasks Persistence that runs as current user for the selected beacon

Meant for quick user level persistence upon initial access


* (ServiceEXEPersist)

Admin Level Custom Service EXE Persistence
    
Runs as elevated user/SYSTEM for the selected beacon



* (WMICEventPersist)
    
Generates a Custom WMI Event using WMIC for SYSTEM Level persistence on selected beacon

Very syntax heavy, Test first before using on live targets


* (StartupGPOPersist)
   
Generates a Local GPO Entry in psscripts.ini to call a .ps1 script file for persistence on selected beacon
   
Calls back as SYSTEM
   
Check permissions with GPO Enumeration (Successful GroupPolicy Directory Listing) first before executing
   
Beacon execution will cause winlogon.exe to hang and the end user can't login. Once the new beacon checks in inject into another process and kill the original. Update to come out soon.


* (RegistryPersist)

Creates a Custom Registry Key, Value, Type, and Payload Location based on user input for selected beacon



* (HKCURunKeyPSRegistryPersist)

Creates two Custom Registry Run Key entries in HKCU
   
The Payload is a base64 encoded powershell payload based off your HTTP/HTTPS listener
 
#(Manual persistence)

is an extension for AM0N-Eye persistence by leveraging the execute_assembly function with the SharpStay .NET assembly.
handles payload creation by reading the template files for a specific execution type. 
The persistence menu will be added to the beacon. Due to the nature of how each technique is different there is only a GUI menu and no beacon commands. 

Available options:

 * ElevatedRegistryKey
 * UserRegistryKey
 * UserInitMprLogonScriptKey
 * ElevatedUserInitKey
 * ScheduledTask
 * ListScheduledTasks
 * ScheduledTaskAction
 * SchTaskCOMHijack
 * CreateService
 * ListRunningServices
 * WMIEventSub
 * GetScheduledTaskCOMHandler
 * JunctionFolder
 * StartupDirectory
 * NewLNK
 * BackdoorLNK
 * ListTaskNames
 
 Dependencies
  * Mono (MCS) for compiling .NET assemblies (Used with dynamic payload creation) 

    
##AVQuery

    Queries the Registry with powershell for all AV Installed on the target
    
    Quick and easy way to get the AV you are dealing with as an attacker
    
##checkmate request 
version of the checkmate request Web Delivery attack


    Stageless Web Delivery using checkmate.exe 
    
    Powerpick is used to spawn checkmate.exe to download the stageless payload on target and execute with rundll32.exe


##Curl-TLS  

simple web requests without establishing SOCKS PROXY. Example use case could be confirming outbound access to specific service before deploying a relay from [F-Secure's C3]


#AV/EDR Recon & EDR exact query
 
As a red-team practitioner, we are often using tools that attempt to fingerprint details about a compromised system, preferably in the most stealthy way possible. Some of our usual tooling for this started getting flagged by EDR products, due to the use of Windows CLI commands.
This aims to solve that problem by only probing the system using native registry queries, no CLI commands.


# Active-Evilentry 
job to execute as your current user context. This job will be executed every time the user logs in. Currently only works on Windows 7, 8, Server 2008, Server 2012.


# BypassUAC-eventvwr
 
silentcleanup UAC bypass that bypasses "always notify" aka the highest UAC setting, even on Windows


#info_Advanced

A common collection of OS commands, and Red Team Tips for when you have no Google or RTFM on hand.


#BOF & (New command)

    AV_Query                  Queries the Registry for AV Installed
    FindModule                Find loaded modules.
    FindProcHandle            Find specific process handles.
    amsi-inject               Bypass AMSI in a remote process with code injection.
    blockdlls                 Block non-Microsoft DLLs in child processes
    bypassuac-eventvwr        Bypass UAC using Eventvwr Fileless UAC bypass via. Powershell SMB Beacon
    cThreadHijack             cThreadHijack: Remote process injection via thread hijacking
    dllinject                 Inject a Reflective DLL into a process
    dllload                   Load DLL into a process with LoadLibrary()
    edr_query                 Queries the remote or local system for all major EDR products installed
    etw                       Start or stop ETW logging.
    execute-assembly          Execute a local .NET program in-memory on target
    info_RTFM                 A large repository of commands and red team tips
    kerberos_ccache_use       Apply kerberos ticket from cache to this session
    kerberos_ticket_purge     Purge kerberos tickets from this session
    kerberos_ticket_use       Apply kerberos ticket to this session
    process-hollowing         EarlyBird process hollowing technique - Spawns a process in a suspended state, injects shellcode, hijack main
    thread with APC, and execute shellcode.
    regenum                   System, AV, and EDR profiling via registry queries
    shinject                  Inject shellcode into a process
    show_beacon_downloads     Show all Downloads associated with your current Beacon.
    show_sync_location        Shows sync location for downloads.
    static_syscalls_apc_shspawnSpawn process and use syscalls to execute custom shellcode launch with Nt functions (NtMapViewOfSection -> NtQueueUserApc).
    static_syscalls_apc_spawn Spawn process and use syscalls to execute beacon shellcode launch with Nt functions (NtMapViewOfSection -> NtQueueUserApc).
    static_syscalls_dump      Use static syscalls to dump a given PID and save to disk
    static_syscalls_inject    Use static syscalls to execute CRT beacon shellcode launch with Nt functions.
    static_syscalls_shinject  Use static syscalls to execute custom shellcode launch with Nt functions.
    sync_all_beacon_downloads Sync all Downloads.
    sync_beacon_downloads     Sync all Downloads from current Beacon.
    syscalls_inject           Use syscalls from on-disk dll to execute CRT beacon shellcode launch with Nt functions.
    syscalls_shinject         Use syscalls from on-disk dll to execute custom shellcode launch with Nt functions.
    unhook                    remove hooks from DLLs in this process
    zerologon                 Reset DC machine account password with CVE-2020-1472
    
    
    __________________________________________________________________________________________________________________________________
    
    
