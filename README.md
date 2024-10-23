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
I will be using primarily Autopsy 4.21.0 (latest version as of 2024-10-22) to investigate this case.

## Walkthrough, Questions, and Answers
Let's start up Autopsy 4.21.0 and set up a case. Click <strong>New Case</strong>.

<img src="./pics/1_Autopsy_Start.png" alt="Start Autopsy">

Set a case name. The additional information like case number is optional. Click <strong>Next</strong> and then <strong>Finish</strong>.
<img src="./pics/2_New Case Fill Out.png" alt="Fill Out New Case">

<img src="./pics/3_New Case Information.png" alt="Optional Additional Case Information">

Add the data source.

<img src="./pics/4_Add_Data_Source.png" alt="Add Data Source">

For Data Source Type, select <strong>Disk Image or VM File</strong>
<img src="./pics/5_Select_Data_Source.png" alt="Select Data Source">

Point to the image. In my case, I downloaded the spanned DD image, so I pointed the path of the first segment. Autopsy will automatically recognize the spanned image when you point to the first segment.
<img src="./pics/6_Add_Data_Source2.png" alt="Select image data source">

On the <bold>Configure Ingest</bold> step, there are multiple ingest modules. Most of them are available for free by default. Note: Cyber Triage Malware Scanner requires a paid license to scan the image for malware. Let's select all to see what happens. 
<img src="./pics/7_Configure_Ingest.png" alt="Configure Ingest">

Click Next.
