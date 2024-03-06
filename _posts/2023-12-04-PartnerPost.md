---
layout: post
title: CTF Walkthrough
type: hacks
courses: { compsci: {week: 2} }
---

<h2>
Description:
</h2>
<h3>
A walkthrough of how to solve the capture-the-flag challenge made by Anvay and Yash
</h3>

<style>
    @keyframes flashPop {
      0% { opacity: 0; transform: scale(1); color: #fcf000; }
      50% { opacity: 1; transform: scale(3.0); color: #fcf000; }
      100% { opacity: 0; transform: scale(2); color: #f70202; }
    }

    @keyframes fadeInText {
      0% { opacity: 0; }
      100% { opacity: 1; }
    }
    
    .flash-pop {
      animation: flashPop 5s linear 2 forwards;
      display: inline-block;
    }
    
    .additional-text {
      opacity: 0;
      animation: fadeInText 6s linear 1 forwards; /* Start after flashPop is complete */
      animation-delay: 10s;
      color: white;
    }

    .additional-text-code {
        color: #00ffcc; /* Cyan text for additional-text-code class */
    }

    pre {
        background-color: #000; /* Black background for code snippets */
        color: #fff; /* White text for code snippets */
        padding: 10px;
        border-radius: 5px;
        overflow: auto;
    }

    .additional-text-green {
        color: #fcf000; /* Green text for additional-text-green class */
    }

</style>

<p class="flash-pop">⚠️⚠️⚠️ SPOILER ALERT: SOLUTION WILL BE SHOWN BELOW ⚠️⚠️⚠️</p>

<h1 class="additional-text">Step 1: Extract hashes from the SAM Database</h1>


<p class="additional-text">
    The SAM database is the file that the Windows operating system uses to store usernames and match them to passwords
</p>

<h2 class="additional-text">Extraction Way #1:</h2>

<p class="additional-text">
    The first way you can extract the passwords from the SAM database is through the registry editor.
    <span class="additional-text-green">Make sure to be safe when navigating the Registry Editor. A single thing messed up in here means that your computer could risk its last use.</span>
</p>
<img class="additional-text" src="{{site.baseurl}}/images/week2/regeditlookup.png">
<p class="additional-text">
    After you have done this, navigate to HKEY_LOCAL_MACHINE and then SAM. Then, right-click and Export the key.
</p>
<img class="additional-text" src="{{site.baseurl}}/images/week2/regbackup.png">
<h2 class="additional-text">Extraction Way #2:</h2>

<p class="additional-text">
    Another way that you can extract the SAM database is through PowerShell.
    PowerShell is a task automation and configuration management program from Microsoft.
</p>

<p class="additional-text">
    To do this, first open PowerShell by pressing the Windows key and typing in PowerShell.
    After opening PowerShell, run the following commands:
</p>

<pre class="additional-text">
reg save HKLM\sam ./sam.save
reg save HKLM\system ./system.save
</pre>

<p class="additional-text">If you used this way to save the SAM database, now you’re ready for Step 2!</p>

<h1 class="additional-text">Step 2: Move all saved SAM databases to a Kali Linux image</h1>

<p class="additional-text">
    Note: To understand what Kali is and how to download it and open it in VMWare, refer to the Intro to CTF post on the Hacks page for week 2.
</p>

<p class="additional-text">Once you have Kali installed and opened, run the following:</p>

<pre class="additional-text">
impacket-secretsdump -sam sam.save -system system.save LOCAL
</pre>

<p class="additional-text">
    The <code class="additional-text">impacket-secretsdump</code> command is part of the Impacket library, which is a collection of Python classes for working with network protocols.
    Specifically, this command is used for extracting password hashes from a Windows system.
</p>

<ul class="additional-text">
    <li><span class="additional-text-code">impacket-secretsdump:</span> This is the command-line utility provided by Impacket for performing various operations related to credential extraction.</li>
    <li><span class="additional-text-code">-sam sam.save:</span> This flag specifies the path to the Security Account Manager (SAM) database file (sam.save in this case).</li>
    <li><span class="additional-text-code">-system system.save:</span> This flag specifies the path to the system registry hive file (system.save in this case).</li>
    <li><span class="additional-text-code">LOCAL:</span> This argument specifies the target; in this case, it indicates that the command should be executed locally on the system.</li>
</ul>
<img class="additional-text" src="{{site.baseurl}}/images/week2/importSAM_linux.png">
<p class="additional-text">
    So, when you run this command, it reads the SAM and system registry hive files from the specified paths (sam.save and system.save),
    extracts password hashes, and outputs the results.
</p>

<h1 class="additional-text">Step 3: Crack the password hashes using hashcat</h1>

<p class="additional-text">
    Using the impacket command, we have extracted the password hashes from the SAM database.
    Now, we need to crack these passwords.
</p>

<p class="additional-text">To do this, run the following command:</p>

<pre class="additional-text">
hashcat -m 1000 hash.txt /rockyou.txt --show
</pre>

<p class="additional-text">
    The <code class="additional-text">hashcat</code> command is a powerful and popular password-cracking tool that supports various hashing algorithms.
    Here's a breakdown of the provided command:
</p>

<ul class="additional-text">
    <li><span class="additional-text-code">-m 1000:</span> This option specifies the hash mode to use. In this case, it indicates mode 1000, which corresponds to the NTLM hashing algorithm commonly used in Windows environments.</li>
    <li><span class="additional-text-code">hash.txt:</span> This is the name of the file containing the target hashes.</li>
    <li><span class="additional-text-code">/rockyou.txt:</span> This is the path to a wordlist file, in this case, rockyou.txt.</li>
    <li><span class="additional-text-code">--show:</span> This option instructs hashcat to display the cracked passwords if it successfully finds matches in the provided wordlist.</li>
</ul>
<img class="additional-text" src="{{site.baseurl}}/images/week2/crackedpwd.png">
<p class="additional-text">
    So, the overall purpose of this command is to use hashcat to attempt to crack password hashes (in NTLM format) stored in the hash.txt file using the passwords from the rockyou.txt wordlist.
    If successful, the tool will display the cracked passwords.
</p>

<p class="additional-text">Congratulations! Using hashcat, you just cracked the password of <code class="additional-text">insecureaccount</code>. Seems like they kept their password as “password”.</p>