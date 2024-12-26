<h1>SOC Automation (Home Lab) - Part 2</h1>

<p align="center">
<img src="https://snipboard.io/d69ogf.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part two of five for the series of the SOC automation project. If you haven't seen part one, where we go over how to build a diagram for this lab. I highly recommend you go and read that. Today, our objective is to install our applications and virtual machines. By the end of the video, you should have one Windows 10 machine with Sysmon installed, one Wazuh server, and one Hive server. 
  
For those who don't know what Wazuh is, their site will provide a very high-level overview. Wazuh is an open-source cyber security platform that integrates SIEM and XDR capabilities in a unique solution. They provide multiple capabilities such as security analytics, intrusion detection, incident response, and many more. They have three main components that build up Wasa, which is the indexer, server, and dashboard. 
<br />
<br />
<img src="https://snipboard.io/Z1a7dV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<img src="https://snipboard.io/dUBCD8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<img src="https://snipboard.io/niqex0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
As for the Hive, this is a 4-in-1 open-source security incident response platform, and we will use this as our case management system. As for our virtual machine, we will be using Windows 10 as our client PC, and I use Ubuntu 22.04 for both Wazuh and the Hive. Again both Wazuh and the Hive will be spun up in the cloud, however, I will be using a virtual machine to set up my Windows 10 client hosted on my ESXi server. You can always use Virtual Box or the cloud and if you aren't too sure how to install Virtual Box and Sysmon to get set up with Windows 10, I'll walk you through it. Otherwise, you can just skip ahead. 
<br />
<br />
<img src="https://snipboard.io/ZlMhSY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
First, we'll begin installing Windows 10 by installing Virtual Box. Head over to their site virtualbox.org. Now depending on the operating system you have that is the one that we will be downloading. So we can go ahead and click download Virtual Box 7.0 from here.
<br />
<br />
<img src="https://snipboard.io/LCJyP5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
We know that this machine that I'm using is a Windows host machine, So I'll go ahead and click "Download" on that.
<br />
<br />
<img src="https://snipboard.io/2lta9c.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
<br />
<br />
While that's downloading, we can go ahead and check out the sha256 checksums as well. When you go into the checksums, it will provide you with a list of sha256 hashes, and then this way we can verify the downloaded file to determine whether or not it has been altered.
<br />
<br />
<img src="https://snipboard.io/Nrctv8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
<img src="https://snipboard.io/vWkugN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
<br />
<br />
So let's head over to the downloads directory in which our file lives. We'll open Powershell and then I'll do a get-file hash. By default, sha256 is generated.
<br />
<br />
<img src="https://snipboard.io/cnwbSi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/0elMb5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/B9H2mL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
What we can do is copy and paste it in and see if it matches up. So we know for a fact that the file in transit was not changed whatsoever. Now we can go ahead and double-click the Virtual box to start installing it.
<br />
<br />
<img src="https://snipboard.io/UdCZ7I.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/2jImHi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/i5xGpb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now, we might get hit with a compatibility issue or missing some software dependencies but we'll deal with that when the time comes. Click on Yes. From here we notice that there is a Microsoft Visual C++ 2019 package dependency. So what we'll do is we'll go ahead and install this dependency and I'll put the link down in the description below just in case you get hit with this dependency as well.
<br />
<br />
<img src="https://snipboard.io/zms016.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/3LfQsU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
<p align="center">
I went and downloaded the dependency and installed it. So now we can go ahead and double-click the virtual box installer again. Hit yes. And now it doesn't give me that dependency error anymore.
<br />
<br />
<img src="https://snipboard.io/QP5hGu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/bYOvWH.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/eATVDh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We are presented with some of the features that we can install and they are all set to be installed by default, but you do have the option to customize it which is quite nice. Here you can change where you want the virtual box to be installed. For example, if you don't have enough space in the C drive, you can always install it in a different drive if you like. Hit next.
<br />
<br />
<img src="https://snipboard.io/bs15qd.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
This will provide you with a warning that it will reset your network connection and temporarily disconnect you. Hit yes and install the needed dependencies.
<br />
<br />
<img src="https://snipboard.io/q1EL2X.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once it's done installing, all you have to do is click on "Finish" and then the virtual box should automatically pop up.
<br />
<br />
<img src="https://snipboard.io/VPRb1A.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yE3TnU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We'll navigate to the link which I'll list down in the description for you, and once we're on this page we can go ahead and scroll down and click on "Download Tool Now" This will download what is called a media creation tool which will help you generate a Windows ISO image file.
<br />
<br />
<img src="https://snipboard.io/2EBXvK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So we can go ahead and double click to open the file. Click on yes. And now it will get a few things ready until you'll eventually be presented with this license agreements page. And we can go ahead and hit "Accept". It will say getting a few things ready again. Once it is done check checking things you should be presented with this screen.
<br />
<br />
<img src="https://snipboard.io/zNC4xh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/kfyKOG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/aoCwKV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Displaying two options. One to upgrade your PC or two create an installation media. What we want to select is "Create the installation media" and hit "Next". You have the option to customize your settings such as language addition and architecture, but I'll leave mine checked to use the recommended option for this PC, and hit "Next".
<br />
<br />
<img src="https://snipboard.io/92kLpB.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/1PJYA4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now we are presented with two media choices. Use a USB flash drive or an ISO file. We'll select the ISO file and hit "Next". Save the ISO file anywhere you like and it will start downloading. Now that my Windows ISO image has been successfully downloaded we can now jump over to our virtual box and start creating a virtual machine.
<br />
<br />
<img src="https://snipboard.io/zKEirl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We'll open up the window for the virtual box and click on "New". Here we can enter a name for a virtual machine and select the directory where we want to store our files. I'll go ahead and name our virtual machine "demo". I'll leave the folder as is for the iso image. I will click on the drop-down. Click "Other" and now I'll find the iso image that I just downloaded.
<br />
<br />
<img src="https://snipboard.io/2FCxSG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
There's an option where you can check "Skip unattended installation" which I will do. That way I can install the operating system manually. You can uncheck this or check it, it's up to you. Click on "Next". 
<br />
<br />
<img src="https://snipboard.io/zo5ZjI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Here we have the option to configure our virtual machine specifications. However, do be aware that this will be relying on your computer's specifications for this demo. I'll set my base memory for this virtual machine as 4 gigs and we'll have it as one CPU. I'll click on "Next" for the virtual hard disk. I'll leave it as 50 gigs and hit "Next".
<br />
<br />
<img src="https://snipboard.io/UdPvM2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/VglP3G.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now this will give you a nice summary as to what your settings are for this virtual machine. If you're good to go click "Finish" and now we can go ahead and start powering it on. To power it on, you just hit the arrow that says "Start Now". once it's running we should be able to start seeing this Windows 10 setup. So we'll go ahead and select "Next".
<br />
<br />
<img src="https://snipboard.io/nzNcx6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/SYOGDK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/z7ZFCK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Hit install. Now once you're presented with this activate Windows screen, go ahead and select, "I don't have a product key". And as for the option to select, "Windows 10 Pro". Hit next. Accept the license terms and here you have the option to upgrade or custom install Windows only. I'm going to select custom install Windows only.
<br />
<br />
<img src="https://snipboard.io/XMrw5n.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EKgVeq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/vAMQ9k.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Accept the license terms and here you have the option to upgrade or custom install Windows only. I'm going to select custom install Windows only. Hit next. And then now with Windows 10 installing in the background, what we want to do is open up
"Install Sysmon", a web browser. 
<br />
<br />
<img src="https://snipboard.io/uipwWl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/L2je1V.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/SiktXU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once you're on this site you want to click on "Download Sysmon" unless you're running Linux, then you can go ahead and click on the link for GitHub, but since I'm on a Windows machine we'll click on download Sysmon. And while that's downloading we should go ahead and start downloading the configuration file that we'll start using for Sysmon. Again all of this will be in the description down below.
<br />
<br />
<img src="https://snipboard.io/SiktXU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once we're on this page, we want to scroll down to find "sysmon.config.xml". Click on that. Once you're on this page, you want to click on raw and then it'll load this up, and then all you have to do is right-click click save as, and then you can save it as anything you want. In this case, I'll just type in "Sysmonconfig" and then save it as that.
<br />
<br />
<img src="https://snipboard.io/iJyhfa.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nLgjdp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/XoCOfs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now that Sysmon has successfully been downloaded, what we can do is start extracting it. So I'll right click it and click "Extract all". What we want to do is not double-click this executable, instead, we want to open up a Powershell window and we might need administrator privileges. So why don't we go ahead and open up a Powershell with admin privileges. So we go to the bottom left corner. Click on the Windows button. Type in Powershell. From here you can click on run as administrator or you can right-click and run as administrator.
<br />
<br />
<img src="https://snipboard.io/XoCOfs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/khoNPj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
Now we want to make sure that we're in the same directory as the extracted Sysmon. So, in this case, the directory is "C:\\User\Bobby\Downloads\Sysmon". Right-click copy and then all we have to do is jump over to the Powershell prompt. Type in "cd" to change the directory, and just for good practice, put it in a quote. So now we should be in the correct directory.
<br />
<br />
<img src="https://snipboard.io/szKvMC.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/fnAT9S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/syZRor.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
What we want to do is also make sure that our Sysmon configuration is in the same directory as well. So in this case, we can either copy it or we can cut it. I'll just drag and drop it into that folder. Double-click it to make sure that it's there and indeed it is there.
<br />
<br />
<img src="https://snipboard.io/sGuQj1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/9Ot4Y0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
You might have noticed that there are various EXE enabled for Sysmon. We want to focus on the Sysmon 64, simply because this is a 64-bit machine. So all we have to do is type in .\Sysmon64.exe. If you hit "Enter". Nothing will happen it will just show you the help menu. Essentially it'll just tell you how to install it and update the configurations and all that other good stuff. 
<br />
<br />
<img src="https://snipboard.io/1qSTia.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/h1pL5J.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
and double-check if we have or have not installed cismon we can do this in a couple ways so first and foremost we can go in the bottom left corner click on the start menu and type in Services hit enter this should pop up the services that are installed onto your computer and then what you want to type in is or actually what you want to click and type is s and then you want to look for cismon if there's no cismon installed then you will not see sysmon so in this case I can confirm that I do not have sysmon installed the second way we can do it is go through event logs we can do this by typing Event Viewer hit enter there this will show you all of the logs associated with this machine so what you want to do is click on applications service logs open that up and then you want to expand Microsoft from there expand windows and now we want to look for cismon so if I scroll all the way down is there going to be any cismon and no doesn't seem that cismon is installed so what we want to do now is Type in cismon again 64 tab now that auto completes and now we want to install the configuration by using the TAC I command right here install service and Driver optionally take a config configuration file so we'll press space and type in Symon and then we can tab until we find it or we can just type in cismon config since we know that we named it that way hit Tab and it'll autoc complete from
