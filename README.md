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
To double-check if we have or have not installed Sysmon. We can do this in a couple of ways. So first and foremost, we can go to the bottom left corner. Click on the start menu and type in "Services". Hit "Enter". This should pop up the services that are installed on your computer. And then what you want to type in is "S" and then you want to look for Sysmon. If there's no Sysmon installed, then you will not see Sysmon. so in this case I can confirm that I do not have Sysmon installed.
<br />
<br />
<img src="https://snipboard.io/6FDOwM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zfPohb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
The second way we can do it is to go through event logs. We can do this by typing "Event Viewer". Hit "Enter". This will show you all of the logs associated with this machine. So what you want to do is click on applications service logs. Open that up and then you want to expand Microsoft. From there, expand windows and now we want to look for Sysmon. If I scroll down, is there going to be any Sysmon, and no doesn't seem that Sysmon is installed.
<br />
<br />
<img src="https://snipboard.io/0ga7pP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/9HU4vf.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/PJGDR8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NPthwz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
So what we want to do now is type in ".\Sysmon64.exe -i .\sysmonconfig.xml". We want to install the configuration by using the TAC I command (Install service and driver optionally take a config configuration file). Hit "Enter" and this will pop up, which is essentially your license and agreement. You can hit agree and then it should go ahead and run its course and install Sysmon.
<br />
<br />
<img src="https://snipboard.io/iZ7NDH.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/AWxBZs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
So to double-check to make sure that Sysmon is installed, we can run through those steps that I've shown previously. Click the Windows button. Type in "Event Viewer". Click on "Applications and service logs" and expand "Microsoft" and then "Windows". Scroll all the way down and see if you have Sysmon. In this case, we do. beautiful So that is how you install Sysmon and get that up and running.
<br />
<br />
<img src="https://snipboard.io/3Kr0St.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/5fHhq8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/hpnZbJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
We can click on "Operational" and this will give you a bunch of telemetry about your system and help you catch evil. Now that we have our Windows 10 client machine installed, along with Sysmon, let's start setting up Wazuh next. I'll be using Wazuh Digital Ocean as my cloud provider. If you want to follow along, you can sign up using the link provided down below. That will provide you with $200 credit for the first 60 days but you must provide a valid credit card.
<br />
<br />
<img src="https://snipboard.io/KV5qep.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
To begin I'm going to be building our Wasa server. First, so we can create that by clicking on the top right corner called "Droplets". From here you want to select the region that you're currently residing in. In my case, I'll just select "San Francisco". Scroll down and we want to select Ubuntu and version 22.041. As for the droplet type, we'll want 50 gigs of RAM and 50 gigs of hard drive space. So we can go with the option of $48 a month. Now again with the $200 credit. This doesn't seem too much.
<br />
<br />
<img src="https://snipboard.io/Ku7SYn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Jp8UOS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/9LrIvF.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zm5QKl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
So this is why I'm going to be choosing, the basic plan, Premium Intel, and the $48 a month. Scroll down. We want to create a password or an SSH key whichever suits you, but for me, I'm just going to create a password. Do make sure that you use a password manager because we will expose this to the internet eventually, but not in the beginning. Once you put in your password, scroll down. 
<br />
<br />
<img src="https://snipboard.io/om2z3c.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
You'll see an option to change your hostname. I'll change this to Wazuh. Scroll down and then hit create droplet at the bottom. While we wait for this droplet to be created, I highly recommend you start set setting up a firewall, otherwise you'll get spammed from external scanners. We can create a firewall by heading over to networking on the left-hand side and then the "Firewalls" tab. Click on "Create Firewall". Now by default, we have SSH open to the public. I'm going to name this as "Firewall". I am going to select all TCP I'll remove all IPS and only add your IP.
<br />
<br />
<img src="https://snipboard.io/mkAaf2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/QfhXTK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/UwQCod.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
Now to get your public IP, open up a new tab and type "whatismyIP address" and then select the first link. You should see your public IP address. Copy and paste it into your "Sources". Now you want to do the same for UDP aswell, just in case. Once that's done. Scroll all the way down and click on "Create Firewall". 
<br />
<br />
<img src="https://snipboard.io/xLSHR5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Z8F0XK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />  
Once your firewall had been created, we can start adding our virtual machine on to this firewall. The reason why we're doing this is, if you don't create rules, by default SSH is open to the public so anybody or any scanners out there will hit your box and try to break in. However, since we specified our public IP, our virtual machine is only accessible through us. Let's head over to our virtual machines by selecting droplets on the left hand side, and here we can see the public ip of our Wazuh server.
<br />
<br />
<img src="https://snipboard.io/Z8F0XK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
Let's click on Wazuh. From here we want to select "Networking". Scroll down until you see firewalls. Click on "Edit" and then you want to select your firewall that you just created. From your firewall, click on "droplets" and select "Add droplets". Look for your virtual machine in my case its Wazuh. I'll select Wazuh and click on "Add droplet". Now the firewall will be protecting our virtual machine.
<br />
<br />
<img src="https://snipboard.io/mftRUc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/QX8nJZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Kin6XW.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/lcsqIg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />  
There are a couple of ways that you can access your virtual machine. Firstly you can use something like Putty to use SSH, or from the "Droplets" tab under Wazuh you can click on the "Access" Tab and then select "Launch Droplet Console". Once we have successfully sshed into our virtual machine, we can begin performing some updates and upgrades. 
<br />
<br />
<img src="https://snipboard.io/UFyoHn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yS4YKn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nLmK0f.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />  
Now because by default we're under root, we can perform this by typing in "apt-get update && at-et upgrade -y". You will eventually get presented with this screen. We can just hit "Enter" and finally It'll ask you which Services should be restarted. Again, we can just hit "Enter". Once it's been finished updating and upgrading, we can start with the installer of Wazuh. To get started with Wazuh, we can run a curl command that is also found on their website, but I'll leave it here as well as in the description down below if you wanted to just copy.
<br />
<br />
<img src="https://snipboard.io/k4NL72.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/R3b7Iz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/1Qhplq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
After a couple of minutes, Wazuh should be finished installing. Now there is one thing to keep in mind and that is the username and password. You want to make sure that you copy the username admin as well as your password, because you will need it to log in to your Wazuh dashboard so let's go and do that right now.
<br />
<br />
<img src="https://snipboard.io/BXFeIq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
To log into Wazuh, do take note of your public IP of the server, in this case, my public IP for Wazuh is "159.203.17.39". I'll go ahead and copy and then open up a new tab and in your browser make sure you type in "https" and then paste in the IP.
<br />
<br />
<img src="https://snipboard.io/pa1i3q.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/xHgOls.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br /> 
Now we're presented with Wazuh's dashboard. We can put in our admin user and paste in the password. Now we're in. Perfect. We have our client machine and Wazuh up and running. The next thing is to install the Hive. Similar to Wazuh, we'll install the Hive by using Ubuntu 22.04. 
<br />
<br />
<img src="https://snipboard.io/l3Hunt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/cNFXOn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To begin, select "Create" at the top right corner and select "Droplet". You want to choose the region that's closest to you. I'll select San Francisco. For the specs, I'll use the 8 gigs, similar to Wazuh. Scrolling down, you can use an SSH key or password. I'll be using a password.
<br />
<br />
<img src="https://snipboard.io/Ntbz03.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/UcHpKr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/P3BCfk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/f8he4T.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
For the hostname, I'll name it "TheHive". Then select "Create Droplet". You want to make sure that the Hive is placed in the firewall that you created earlier. To do that we can click on "TheHive". Go into "Networking". Scroll down to "Firewalls". 
<br />
<br />
<img src="https://snipboard.io/GLyHW5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/hCUpK8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/vq1eNh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/KsrxAI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Click on your firewall. Go to Droplet and then "Add Droplet". Now we want to type in our hostname, which is "TheHive" and click on "Add Droplet". Perfect. Now both Wazuh and the Hive is being protected by our firewall and it can only be accessible by Us and nobody else.
<br />
<br />
<img src="https://snipboard.io/qYvw1Q.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/fzAqZJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/7GT1nP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/2SIVbT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I've opened up a new tab and SSH'd into the Hive. Now we can start installing some prerequisites. Again this could be found in their documentation and I will also leave it in the description so you can just easily copy and paste. With the Hive we must install four components. The first one is Java. Second is Cassandra. Third is Elastic Search, and fourth, is the Hive itself.
<br />
<br />
<img src="https://snipboard.io/P1dhDQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/RuNfe2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once we finish installing the prerequisites we can start installing Java. Whenever you are presented with this screen, you can go ahead and hit "Enter". Once Java has finished installing, the next one is Cassandra. Then we can install Elastic Search, and finally, we can install the Hive. This whole process of installing the Hive and Wazuh takes about 10 to 15 minutes, but once it's finished installing, the next step is to configure it.
<br />
<br />
<img src="https://snipboard.io/Do9RNs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/RuNfe2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Hqp6OJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now that we have installed our virtual machines Wazuh and the Hive, we must configure these to get them to work across the board, and we will do this in the next episode.
