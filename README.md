# NIST Hacking Case 2004-09-20

This is the original link to the hacking case: https://cfreds-archive.nist.gov/Hacking_Case.html

## Background Information
This hacking case is one of several Computer Forensic Reference Data Sets (CFReDS) provided by the US government's NIST (National Institute of Standards and Technology) to aid investigators in:
<ul>
    <li>validating software tools used in their investigations</li>
    <li>checking equipment</li>
    <li>training investigators</li>
    <li>testing investigators on proficiency for lab accreditation</li>
</ul>

## Scenario 
On 09/20/04 , a Dell CPi notebook computer, serial # VLQLW, was found abandoned along with a wireless PCMCIA card and an external homemade 802.11b antennae. It is suspected that this computer was used for hacking purposes, although cannot be tied to a hacking suspect, G=r=e=g S=c=h=a=r=d=t. (The equal signs are just to prevent web crawlers from indexing this name; there are no equal signs in the image files.)  Schardt also goes by the online nickname of “Mr. Evil” and some of his associates have said that he would park his vehicle within range of Wireless Access Points (like Starbucks and other T-Mobile Hotspots) where he would then intercept internet traffic, attempting to get credit card numbers, usernames & passwords.

Find any hacking software, evidence of their use, and any data that might have been generated. Attempt to tie the computer to the suspect, G=r=e=g S=c=h=a=r=d=t.

## Tools
I will be using the free open-source tool Autopsy 4.21.0 (latest version as of 2024-10-22) to investigate this case.

## Initial Walkthrough and Case Setup
Let's start up Autopsy 4.21.0 and set up a case. Click <strong>New Case</strong>.

<img src="./pics/1_Autopsy_Start.png" alt="Start Autopsy">

Set a case name. The additional information like case number is optional. Click <strong>Next</strong> and then <strong>Finish</strong>.

<img src="./pics/2_New Case Fill Out.png" alt="Fill Out New Case">

<img src="./pics/3_New Case Information.png" alt="Optional Additional Case Information">

Add the data source. Click <strong>Next</strong>.

<img src="./pics/4_Add_Data_Source.png" alt="Add Data Source">

For Data Source Type, select <strong>Disk Image or VM File</strong>. Click <strong>Next</strong>.

<img src="./pics/5_Select_Data_Source.png" alt="Select Data Source">

Point to the image. In my case, I downloaded the spanned DD image, so I pointed the path of the first segment. Autopsy will automatically recognize the spanned image when you point to the first segment. Click <strong>Next</strong>.

<img src="./pics/6_Add_Data_Source2.png" alt="Select image data source">

On the <bold>Configure Ingest</bold> step, there are multiple ingest modules. Most of them are available for free by default. Note: Cyber Triage Malware Scanner requires a paid license to scan the image for malware. Let's select all to see what happens. Click <strong>Next</strong>.

<img src="./pics/7_Configure_Ingest.png" alt="Configure Ingest">

Click <strong>Finish</strong> on the last step.

<img src="./pics/7_Data_Source_Added.png" alt="Data Source Added">

Autospy will then begin ingesting. Note the bottom right corner showing the ingest progress.

<img src="./pics/8_Ingesting.png" alt="Ingest">

After the ingest, you can click on the bottom right and see the errors and findings post-ingest.

<img src="./pics/9_Finished_Ingesting.png" alt="Post-Ingest Errors">

## Questions and Answers
### 1. What is the image hash? Does the acquisition and verification hash match?
The image hashes by algorithm are the following:<br>
MD5: aee4fcd9301c03b3b054623ca261959a<br>
SHA1: da2fe30fe21711edf42310873af475859a68f300<br>
SHA-256: 65e2002fed0b286f49541c7e97dcec0dda913d51a063ceeed86782bdacda2312<br>

The acquisition log provided by NIST does not appear to have the image hash of the entire image. The hash provided by NIST is the MD5 hash value of AEE4FCD9301C03B3B054623CA261959A, which is a match.

<img src="./pics/10_Acquisition_Log.png" alt="Acquisiton Log">

There are different ways to find the image hash. One way is to simply click on the image in the Results Viewer and then checking the bottom pane under the <strong>File Metadata</strong> tab.

<img src="./pics/10_Image_Results_Viewer_Bottom.png" alt="Results Viewer File Metadata on Image">

Another method is to click right-click on the image from the <strong>Data Sources</strong> node and select the <strong>View Summary Information</strong> option.

<img src="./pics/10_View_Summary.png" alt="View Summary">

Then select the image and go to the <strong>Container</strong> tab.

<img src="./pics/10_View_Summary_Click_Image.png" alt="Click Image in View Summary Information">
<img src="./pics/10_View_Summary_Container.png" alt="Container Tab in View Summary Information">

### 2. What operating system was used on the computer?
Windows XP

Autopsy automatically parses this information under Tree Viewer's <strong>Data Artifacts</strong> node: <strong>Operating System Information</strong>

<img src="./pics/11_OS_Information.png" alt="OS Information">

If Autopsy didn't automatically provide this as an artifact, we can also look for the information in the SOFTWARE registry hive:<br>
<strong>\img_SCHARDT.001\vol_vol2\WINDOWS\system32\config\software</strong>

The operating system information should be under the <strong>CurrentVersion</strong> registry key within the SOFTWARE hive:<br>
<strong>software\Microsoft\Windows NT\CurrentVersion</strong>

We can navigate to the registry key by selecting the <strong>Application</strong> tab in the Results Viewer after selecting the SOFTWARE hive file.

<img src="./pics/11_SOFTWARE_Registry_Hive_OS_Info.png" alt="OS Info in SOFTWARE Registry">

According to <a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/overview-of-the-boot-ini-file" target="_blank">Microsoft</a></span>:<br>
<em>The <strong>Boot.ini</strong> file is a text file that contains the boot options for computers with BIOS firmware running NT-based operating system prior to Windows Vista. It's located at the root of the system partition, typically <strong>c:\Boot.ini</strong>.</em>

For operating systems before Windows Vista (2006), we can use the <strong>boot.ini</strong> file to determine the operating system.

<img src="./pics/11_OS_Information_boot.ini_file.png" alt="boot.ini file">

### 3. When was the install date?
Thursday, August 19, 2004 10:48:27 PM GMT or
<br>
Thursday, August 19, 2004 5:48:27 PM GMT-05:00 (local US Central Time)

The install date can be found in the <strong>InstallDate</strong> value, which is also within the <strong>CurrentVersion</strong> registry key within the SOFTWARE hive.

<img src="./pics/12_SOFTWARE_Registry_Hive_OS_Info_InstallDate.png" alt="InstallDate in SOFTWARE Registry">

The install date is provided in both hexadecimal and decimal form. Note the 0x that precedes the numbers. This signifies hexadecimal form.<br>
The number in parentheses represents the decimal number in UNIX time. UNIX time is a date and time representation widely used in computing that measures the time elapsed since 00:00:00 UTC on 1 January 1970, the Unix epoch.

To convert to human-readable date, we need to convert epoch from seconds to human date. A website that can help with converting to human date is https://www.epochconverter.com/
<br>
<img src="./pics/12_Epoch_Converter.png" alt="Epoch Conversion to Human Date">

### 4. What is the timezone settings?
Central Daylight Time (-05hrs GMT)

To find the Time Zone Information, we can look at the <strong>TimeZoneInformation</strong> registry value within the SYSTEM registry hive.

The SYSTEM hive is in the following location:<br>
<strong>\img_SCHARDT.001\vol_vol2\WINDOWS\system32\config\system</strong>

After selecting the SYSTEM hive file, we can navigate to the <strong>TimeZoneInformation</strong> registry value under the <strong>Application</strong> tab:
<strong>system\CurrentControlSet</strong>[00X]<strong>\Control\TimeZoneInformation</strong>

Note: It's common to see two Control Sets, <strong>ControlSet001</strong> and <strong>ControlSet002</strong>. In most cases (but not always), <strong>ControlSet001</strong> will point to the Control Set that the machine booted with, and <strong>ControlSet002</strong> will be the last known good configuration. 

<img src="./pics/13_SYSTEM_Registry_Hive_TimeZone.png" alt="TimeZoneInformation in SYSTEM Registry">

### 5. Who is the registered owner?
Greg Schardt

The registered owner can also be found under the SOFTWARE hive's <strong>CurrentVersion</strong> registry key but this time under the <strong>RegisteredOwner</strong> value:<br>
<strong>software\Microsoft\Windows NT\CurrentVersion</strong>

<img src="./pics/14_SOFTWARE_Registry_Hive_RegisteredOwner.png" alt="RegisteredOwner in SOFTWARE Registry">

### 6. What is the computer account name?

Mr. Evil

Autopsy automatically parses the SAM registry and provides the accounts information under the <strong>OS Accounts</strong> node in the Tree Viewer. 

User ID above 1000, ie., 1003, means a custom-made account, not a default one.

Based on the User ID, login count, and description, "Mr. Evil" is the only valid custom-made user account. The other account with a User ID greater than 1000 is the vendor's account and has 0 login counts.

<img src="./pics/15_OS_Accounts.png" alt="OS Accounts">
<img src="./pics/15_OS_Accounts_Description.png" alt="OS Accounts Descriptions">

The computer account name can be found in the SAM registry hive as well as the SOFTWARE hive.

The SAM (Security Account Manager) registry hive contains user account, login, and group information.

The SAM registry hive is in the following location:<br>
<strong>\img_SCHARDT.001\vol_vol2\WINDOWS\system32\config\SAM</strong>

We can navigate to the Users key:<br>
<strong>SAM\Domains\Account\Users</strong>

Unfortunately, Autopsy's <strong>Application</strong> view does not provide much information on the SAM registry hive's <strong>Users</strong> key. However, all the essential information can be seen in the aforementioned <strong>OS Accounts</strong> node in the Tree Viewer.

<img src="./pics/15_SAM_Registry_Users.png" alt="SAM Registry Users">

We can also look in the SOFTWARE hive's <strong>Winlogon</strong> key under the <strong>DefaultUserName</strong> value:<br>
<strong>software\Microsoft\Windows NT\CurrentVersion\Winlogon</strong>

<img src="./pics/15_SOFTWARE_Registry_Hive_WinLogon_DefaultUser.png" alt="DefaultUserName in SOFTWARE Registry">

### 7. What is the primary domain name?
N-1A9ODN6ZXK4LQ

Domain name or workgroup is a subset of the computer name.

The computer account name can be found under the SYSTEM hive's <strong>ComputerName</strong> key:<br>
<strong>system\CurrentControlSet</strong>[00X]<strong>\Control\ComputerName</strong>

<img src="./pics/15_SYSTEM_Registry_Hive_ComputerName.png" alt="ComputerName in SYSTEM Registry">

The same domain name can be found under the SOFTWARE hive's <strong>Winlogon</strong> key under the <strong>DefaultDomainName</strong> value:<br>
<strong>software\Microsoft\Windows NT\CurrentVersion\Winlogon</strong>

<img src="./pics/15_SOFTWARE_Registry_Hive_WinLogon_DomainName.png" alt="DefaultDomainName in SOFTWARE Registry">

### 8. When was the last recorded computer shutdown date/time?
### 9. How many accounts are recorded (total number)?
### 10. What is the account name of the user who mostly uses the computer?
### 11. Who was the last user to logon to the computer?
### 12. A search for the name of “G=r=e=g S=c=h=a=r=d=t” reveals multiple hits. One of these proves that G=r=e=g S=c=h=a=r=d=t is Mr. Evil and is also the administrator of this computer. What file is it? What software program does this file relate to?
### 13.  List the network cards used by this computer
### 14. This same file reports the IP address and MAC address of the computer. What are they?
### 15. An internet search for vendor name/model of NIC cards by MAC address can be used to find out which network interface was used. In the above answer, the first 3 hex characters of the MAC address report the vendor of the card. Which NIC card was used during the installation and set-up for LOOK@LAN?
### 16. Find 6 installed programs that may be used for hacking.
### 17. What is the SMTP email address for Mr. Evil?
### 18. What are the NNTP (news server) settings for Mr. Evil?
### 19. What two installed programs show this information?
### 20. List 5 newsgroups that Mr. Evil has subscribed to?
### 21. A popular IRC (Internet Relay Chat) program called MIRC was installed.  What are the user settings that was shown when the user was online and in a chat channel?
### 22. This IRC program has the capability to log chat sessions. List 3 IRC channels that the user of this computer accessed.
### 23. Ethereal, a popular “sniffing” program that can be used to intercept wired and wireless internet packets was also found to be installed. When TCP packets are collected and re-assembled, the default save directory is that users \My Documents directory. What is the name of the file that contains the intercepted data?
### 24. Viewing the file in a text format reveals much information about who and what was intercepted. What type of wireless computer was the victim (person who had his internet surfing recorded) using?
### 25. What websites was the victim accessing?
### 26. Search for the main users web based email address. What is it?
### 27. Yahoo mail, a popular web based email service, saves copies of the email under what file name?
### 28. How many executable files are in the recycle bin?
### 29. Are these files really deleted?
### 30. How many files are actually reported to be deleted by the file system?
### 31. Perform a Anti-Virus check. Are there any viruses on the computer?
