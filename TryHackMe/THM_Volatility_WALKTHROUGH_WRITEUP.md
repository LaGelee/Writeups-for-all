# [Volatility](https://tryhackme.com/room/bpvolatility)
### [TryHackMe](https://tryhackme.com/)
Learn how to perform memory forensics with Volatility!

### Stats 
Sections  :   **5**

Questions :   **18**

## [Task 1] Intro

### 1.1) Install Volatility onto your workstation of choice or use the provided virtual machine. On Debian-based systems such as Kali this can be done via "apt-get install volatility"
To install Volatility you can download the project from [Github](https://github.com/volatilityfoundation/volatility.git) and then run the setup.py file. After that you will be able tu use volatility.
```bash
$ git clone https://github.com/volatilityfoundation/volatility.git && cd volatility && python setup.py
```
```bash
$ vol.py                                                                                      
Volatility Foundation Volatility Framework 2.6.1
ERROR   : volatility.debug    : You must specify something to do (try -h)
```
#### `Answer : No answser needed`

## [Task 2] Obtaining Memory Samples

### 2.1) What memory format is the most common?
Just read the intro of the section **"These tools will typically output a .raw file which contains an image of the system memory. The .raw format is one of the most common memory file types you will see in the wild."**
#### `Answer : .raw`

### 2.2) The Window's system we're looking to perform memory forensics on was turned off by mistake. What file contains a compressed memory image?
Same as before : **"hiberfil.sys, better known as the Windows hibernation file contains a compressed memory image from the previous boot. Microsoft Windows systems use this in order to provide faster boot-up times, however, we can use this file in our case for some memory forensics!"**
#### `Answer : hiberfil.sys`

### 2.3) How about if we wanted to perform memory forensics on a VMware-based virtual machine?
Always the same... **"Things get even more exciting when we start to talk about virtual machines and memory captures. Here's a quick sampling of the memory capture process/file containing a memory image for different virtual machine hypervisors:
	VMware - .vmem file
    Hyper-V - .bin file
    Parallels - .mem file
    VirtualBox - .sav file \*This is only a partial memory file. You'll need to dump memory like a normal bare-metal system for this hypervisor"**
#### `Answer : .vmem`

## [Task 3] Examining Our Patient

### 3.1) First, let's figure out what profile we need to use. Profiles determine how Volatility treats our memory image since every version of Windows is a little bit different. Let's see our options now with the command "volatility -f MEMORY_FILE.raw imageinfo"
Read the question.
#### `Answer : No answer needed`

### 3.2) Running the imageinfo command in Volatility will provide us with a number of profiles we can test with, however, only one will be correct. We can test these profiles using the pslist command, validating our profile selection by the sheer number of returned results. Do this now with the command "volatility -f MEMORY_FILE.raw --profile=PROFILE pslist". What profile is correct for this memory image?
Let's download the attached file and unzip it. Now we can start our investigations by finding the profile we need to use with Volatility.
```bash
$ vol.py -f cridex.vmem imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : WinXPSP2x86, WinXPSP3x86 (Instantiated with WinXPSP2x86)
                     AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/alexandre/Bureau/CTF/cridex.vmem)
                      PAE type : PAE
                           DTB : 0x2fe000L
                          KDBG : 0x80545ae0L
          Number of Processors : 1
     Image Type (Service Pack) : 3
                KPCR for CPU 0 : 0xffdff000L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2012-07-22 02:45:08 UTC+0000
     Image local date and time : 2012-07-21 22:45:08 -0400

```
Now we know that after we will have tu use "WinXPSP2x86" profile
#### `Answer : WinXPSP2x86`

### 3.3) Take a look through the processes within our image. What is the process ID for the smss.exe process? If results are scrolling off-screen, try piping your output into less
To inspect the cridex.vnem with volatility we need to specify the profile with "--profile=" and the command "pslist".
```bash
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 pslist
Volatility Foundation Volatility Framework 2.6.1
Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                          
---------- -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0x823c89c8 System                    4      0     53      240 ------      0                                                              
0x822f1020 smss.exe                368      4      3       19 ------      0 2012-07-22 02:42:31 UTC+0000                                 
0x822a0598 csrss.exe               584    368      9      326      0      0 2012-07-22 02:42:32 UTC+0000                                 
0x82298700 winlogon.exe            608    368     23      519      0      0 2012-07-22 02:42:32 UTC+0000                                 
0x81e2ab28 services.exe            652    608     16      243      0      0 2012-07-22 02:42:32 UTC+0000                                 
0x81e2a3b8 lsass.exe               664    608     24      330      0      0 2012-07-22 02:42:32 UTC+0000                                 
0x82311360 svchost.exe             824    652     20      194      0      0 2012-07-22 02:42:33 UTC+0000                                 
0x81e29ab8 svchost.exe             908    652      9      226      0      0 2012-07-22 02:42:33 UTC+0000                                 
0x823001d0 svchost.exe            1004    652     64     1118      0      0 2012-07-22 02:42:33 UTC+0000                                 
0x821dfda0 svchost.exe            1056    652      5       60      0      0 2012-07-22 02:42:33 UTC+0000                                 
0x82295650 svchost.exe            1220    652     15      197      0      0 2012-07-22 02:42:35 UTC+0000                                 
0x821dea70 explorer.exe           1484   1464     17      415      0      0 2012-07-22 02:42:36 UTC+0000                                 
0x81eb17b8 spoolsv.exe            1512    652     14      113      0      0 2012-07-22 02:42:36 UTC+0000                                 
0x81e7bda0 reader_sl.exe          1640   1484      5       39      0      0 2012-07-22 02:42:36 UTC+0000                                 
0x820e8da0 alg.exe                 788    652      7      104      0      0 2012-07-22 02:43:01 UTC+0000                                 
0x821fcda0 wuauclt.exe            1136   1004      8      173      0      0 2012-07-22 02:43:46 UTC+0000                                 
0x8205bda0 wuauclt.exe            1588   1004      5      132      0      0 2012-07-22 02:44:01 UTC+0000
```
So the the PID of the process named "smss.exe" is 368
#### `Answer : 368`

### 3.4) In addition to viewing active processes, we can also view active network connections at the time of image creation! Let's do this now with the command "volatility -f MEMORY_FILE.raw --profile=PROFILE netscan". Unfortunately, something not great is going to happen here due to the sheer age of the target operating system as the command netscan doesn't support it.
Read the question.
#### `Answer : No answer needed`

### 3.5) It's fairly common for malware to attempt to hide itself and the process associated with it. That being said, we can view intentionally hidden processes via the command "psxview". What process has only one 'False' listed?
Like it said just above we need to run "psxview".
```bash
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 psxview
Volatility Foundation Volatility Framework 2.6.1
Offset(P)  Name                    PID pslist psscan thrdproc pspcid csrss session deskthrd ExitTime
---------- -------------------- ------ ------ ------ -------- ------ ----- ------- -------- --------
0x02498700 winlogon.exe            608 True   True   True     True   True  True    True     
0x02511360 svchost.exe             824 True   True   True     True   True  True    True     
0x022e8da0 alg.exe                 788 True   True   True     True   True  True    True     
0x020b17b8 spoolsv.exe            1512 True   True   True     True   True  True    True     
0x0202ab28 services.exe            652 True   True   True     True   True  True    True     
0x02495650 svchost.exe            1220 True   True   True     True   True  True    True     
0x0207bda0 reader_sl.exe          1640 True   True   True     True   True  True    True     
0x025001d0 svchost.exe            1004 True   True   True     True   True  True    True     
0x02029ab8 svchost.exe             908 True   True   True     True   True  True    True     
0x023fcda0 wuauclt.exe            1136 True   True   True     True   True  True    True     
0x0225bda0 wuauclt.exe            1588 True   True   True     True   True  True    True     
0x0202a3b8 lsass.exe               664 True   True   True     True   True  True    True     
0x023dea70 explorer.exe           1484 True   True   True     True   True  True    True     
0x023dfda0 svchost.exe            1056 True   True   True     True   True  True    True     
0x024f1020 smss.exe                368 True   True   True     True   False False   False    
0x025c89c8 System                    4 True   True   True     True   False False   False    
0x024a0598 csrss.exe               584 True   True   True     True   False True    True
```
And we can spot "csrss.exe" with only 1 False.
#### `Answer : csrss.exe`

### 3.6) In addition to viewing hidden processes via psxview, we can also check this with a greater focus via the command 'ldrmodules'. Three columns will appear here in the middle, InLoad, InInit, InMem. If any of these are false, that module has likely been injected which is a really bad thing. On a normal system the grep statement above should return no output. Which process has all three columns listed as 'False' (other than System)?
We can also run "ldrmodules" to view if a process has been injected by something malicious.
```bash
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 ldrmodules | grep False
Volatility Foundation Volatility Framework 2.6.1
       4 System               0x7c900000 False  False  False \WINDOWS\system32\ntdll.dll
     368 smss.exe             0x48580000 True   False  True  \WINDOWS\system32\smss.exe
     584 csrss.exe            0x00460000 False  False  False \WINDOWS\Fonts\vgasys.fon
     584 csrss.exe            0x4a680000 True   False  True  \WINDOWS\system32\csrss.exe
     608 winlogon.exe         0x01000000 True   False  True  \WINDOWS\system32\winlogon.exe
     652 services.exe         0x01000000 True   False  True  \WINDOWS\system32\services.exe
     664 lsass.exe            0x01000000 True   False  True  \WINDOWS\system32\lsass.exe
     824 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
     908 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
    1004 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
    1056 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
    1220 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
    1484 explorer.exe         0x01000000 True   False  True  \WINDOWS\explorer.exe
    1512 spoolsv.exe          0x01000000 True   False  True  \WINDOWS\system32\spoolsv.exe
    1640 reader_sl.exe        0x00400000 True   False  True  \Program Files\Adobe\Reader 9.0\Reader\reader_sl.exe
     788 alg.exe              0x01000000 True   False  True  \WINDOWS\system32\alg.exe
    1136 wuauclt.exe          0x00400000 True   False  True  \WINDOWS\system32\wuauclt.exe
    1588 wuauclt.exe          0x00400000 True   False  True  \WINDOWS\system32\wuauclt.exe

```
So other than "System" there is "csrss.exe" with all the 3 columns as False.
#### `Answer : csrss.exe`

### 3.7) Processes aren't the only area we're concerned with when we're examining a machine. Using the 'apihooks' command we can view unexpected patches in the standard system DLLs. If we see an instance where Hooking module: <unknown> that's really bad. This command will take a while to run, however, it will show you all of the extraneous code introduced by the malware.
You can also run the "apihooks" command if you want.
#### `Answer : No answer needed`

### 3.8) Injected code can be a huge issue and is highly indicative of very very bad things. We can check for this with the command "malfind". Using the full command "volatility -f MEMORY_FILE.raw --profile=PROFILE malfind -D <Destination Directory>" we can not only find this code, but also dump it to our specified directory. Let's do this now! We'll use this dump later for more analysis. How many files does this generate?
We can find and dump malware code with Volatility using "malfind -D ./". Here I will use a "hack" directory to dump the files
```bash
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 malfind -D ./hack && ls ./hack

[...]

process.0x81e7bda0.0x3d0000.dmp    process.0x82298700.0x4c540000.dmp  process.0x82298700.0x554c0000.dmp  process.0x82298700.0x73f40000.dmp
process.0x821dea70.0x1460000.dmp   process.0x82298700.0x4dc40000.dmp  process.0x82298700.0x5de10000.dmp  process.0x82298700.0xf9e0000.dmp
process.0x82298700.0x13410000.dmp  process.0x82298700.0x4ee0000.dmp   process.0x82298700.0x6a230000.dmp  process.0x822a0598.0x7f6f0000.dmp
```
You can count that we have 12 files.
#### `Answer : 12`

### 3.9) Last but certainly not least we can view all of the DLLs loaded into memory. DLLs are shared system libraries utilized in system processes. These are commonly subjected to hijacking and other side-loading attacks, making them a key target for forensics. Let's list all of the DLLs in memory now with the command "dlllist"
DLL is an huge way to inject code and malwares. We can list them using "dlllist"
```
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 dlllist
```
#### `Answer : No answer needed`

### 3.10) Now that we've seen all of the DLLs running in memory, let's go a step further and pull them out! Do this now with the command "volatility -f MEMORY_FILE.raw --profile=PROFILE --pid=PID dlldump -D <Destination Directory>" where the PID is the process ID of the infected process we identified earlier (questions five and six). How many DLLs does this end up pulling?
We can dump DLLs using "dlldump". We know that the "csrss.exe" processus is a malware and that his PID is 584 so let's dumps his DLLs in a "dll" folder
```bash
$ vol.py -f cridex.vmem --profile=WinXPSP2x86 --pid=584 dlldump -D ./dll
Volatility Foundation Volatility Framework 2.6.1
Process(V) Name                 Module Base Module Name          Result
---------- -------------------- ----------- -------------------- ------
0x822a0598 csrss.exe            0x04a680000 csrss.exe            OK: module.584.24a0598.4a680000.dll
0x822a0598 csrss.exe            0x07c900000 ntdll.dll            OK: module.584.24a0598.7c900000.dll
0x822a0598 csrss.exe            0x075b40000 CSRSRV.dll           OK: module.584.24a0598.75b40000.dll
0x822a0598 csrss.exe            0x077f10000 GDI32.dll            OK: module.584.24a0598.77f10000.dll
0x822a0598 csrss.exe            0x07e720000 sxs.dll              OK: module.584.24a0598.7e720000.dll
0x822a0598 csrss.exe            0x077e70000 RPCRT4.dll           OK: module.584.24a0598.77e70000.dll
0x822a0598 csrss.exe            0x077dd0000 ADVAPI32.dll         OK: module.584.24a0598.77dd0000.dll
0x822a0598 csrss.exe            0x077fe0000 Secur32.dll          OK: module.584.24a0598.77fe0000.dll
0x822a0598 csrss.exe            0x075b50000 basesrv.dll          OK: module.584.24a0598.75b50000.dll
0x822a0598 csrss.exe            0x07c800000 KERNEL32.dll         OK: module.584.24a0598.7c800000.dll
0x822a0598 csrss.exe            0x07e410000 USER32.dll           OK: module.584.24a0598.7e410000.dll
0x822a0598 csrss.exe            0x075b60000 winsrv.dll           OK: module.584.24a0598.75b60000.dll
```
And we have 12 files
#### `Answer : 12`

## [Task 4] Post Actions

### 4.1) Upload the extracted files to VirusTotal for examination.
VirusTotal is a website that will analyze the uploaded file and tell you if this is a virus for some antivirus.
#### `Answer : No answer needed`

### 4.2) Upload the extracted files to Hybrid Analysis for examination - Note, this will also upload to VirusTotal but for the sake of demonstration we have done this separately.
Hybrid Analysis will also analyze uploaded file.
#### `Answer : No answer needed`

### 4.3) What malware has our sample been infected with? You can find this in the results of VirusTotal and Hybrid Anaylsis. 
After all of that and looking at the reports we know that we have been infected by "Cridex"
#### `Answer : Cridex`

## [Task 5] Extra Credit

### 5.1) Check out the resources provided above!
Read....
#### `Answer : No answer needed`

Write-up made wit :heart: by @LaGelee