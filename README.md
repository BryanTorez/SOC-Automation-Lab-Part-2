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
We know that this machine that I'm using is a Windows host machine, So I'll go ahead and click "Download" on that. While that's downloading we can actually go ahead and check out the sha256 check sums as well. 
 <br />
<br />
<img src="https://snipboard.io/LCJyP5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
  
  When you go into the check sums it will provide you with a list of Shaw 256 hashes and then this way we can verify the downloaded file to determine whether or not it has been altered so let's go ahead and do that we'll head over to the downloads directory in which our file lives in we'll open Powershell and then I'll do a get file hash virtual box hit tab click on enter and by default sh 256 is generated what we can
<br />
<br />
<img src="https://snipboard.io/TEnY3O.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
To create our diagram, I'll use a site called draw.io because it is free to use and quick to access. The one thing to keep in mind is that these diagrams do not need to be pretty. You just need to do it and build it out. Now later down the road you can go ahead and make it look pretty, especially if you are going to present this to a client, but if it's just for us in a lab environment. I'd rather you put in the work and focus on doing it, versus spending hours trying to make it look visually appealing.
<br />
<br />
<p align="center">
We'll start off by heading over to their site at draw.io.
<br />
<br />
<img src="https://snipboard.io/ldmF1R.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Under the search shapes, we can search for "PC" and hit enter. Now we can select any icon that you like in my case I'm going to select this PC over here. Now again, do keep in mind this does not need to be pretty, just as long as you create it and work out the workflow, you should be okay. 
<br />
<br />
<img src="https://snipboard.io/Sdpij9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/m9yMcp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, I will search for "router" and hit enter. Then let's take this icon. 
<br />
<br />
<img src="https://snipboard.io/bkQRZE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yulB7v.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
I will double-click the PC icon and type "Windows 10 client". We do know for a fact, that this will have the Wazuh agent installed.
<br />
<br />
<img src="https://snipboard.io/FWc2Ay.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, I will search for "internet" and I'll just take the first icon that's the cloud. I'll double-click it and then I'll type internet that way I just know what that is and rotor as well 
<br />
<br />
<img src="https://snipboard.io/zDlCpx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/WeChSL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now because Wazuh, The Hive, and Shuffle is going to live on the internet. I'm going to click on the internet icon, right-click and click "Duplicate". I'll duplicate this three times and I'll rename these as "Wazuh Manager", "The Hive", and lastly, I'll name this as "Shuffle".
<br />
<br />
<img src="https://snipboard.io/eYFzw4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The next thing is the SOC analyst. I'm going to duplicate this PC and I'll just leave this over here and type in "SOC Analyst". 
<br />
<br />
<img src="https://snipboard.io/8zejAV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now here's the fun part. We need to draw fancy lines to connect everything together. That way we know logically, how data will flow. We can include lines by clicking and dragging on the icon itself. 
<br />
<br />
<img src="https://snipboard.io/8zejAV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />  
<br />
<br />
So if you noticed my computer currently doesn't have an arrow pointing towards it we can just change this by clicking on "None" and one of these arrows. Now we can see that there's an arrow icon pointing to my computer icon.
<br />
<br />
<img src="https://snipboard.io/GnIKw1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Ju3IiQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/VNxKsC.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
However, for this example, I am going to select the arrow type. Which is the first option and then select the second one. This will change the arrow icon into what essentially is a link, and I will change the color to let's say gray for now. 
<br />
<br />
<img src="https://snipboard.io/xLipG6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/KChZgv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
<br />
<br />
The gray link is going to indicate the sending of events over to the Wazuh manager. Now I will double-click on the link and then type in "Send Events". Let's create another link from the router over to the internet, and again to do that we can click on the arrow, drag it over to the internet and then I will change the arrow type to link and then change the color to gray.
<br />
<br />
<img src="https://snipboard.io/q7XglG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/o6xvOk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />  
<br />
<br />
Now I'm not going to enter in a text because I know for a fact that that the gray link is sending events. I'm going to move the Wazuh manager icon over just a little bit that way we have a little bit of room to play around with. Same with the Hive and Shuffle.
<br />
<br />
<img src="https://snipboard.io/rzdBiv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />  
<br />
<br />
Perfect. Now I will connect the link from the internet to the Wazuh manager. Change the line to link. Change my color. And in this case, I will type in "Receive Events". I'm also going to put in the steps. So step number one, send events. Step number two, receive events. And then from the Wazuh manager, we are going to be sending it over to shuffle.
<br />
<br />
<img src="https://snipboard.io/pwtdNa.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Yg620m.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /> 
<br />
<br />
Now I will change this to a link because I don't like how the line is not straight. You can select the option called "Waypoints" and then you just select the first one and now it is straight.
<br />
<br />
<img src="https://snipboard.io/ECQqVR.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ce2XRb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/eE4AYc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now if we think logically about how our workflow will work, is that the computer will be sending events over to the Wazuh manager. Wazuh's manager will then create an alert and send that over to Shuffle. In the instance of that, I want to make sure that the link color is not gray that way we can identify a different action. I'll just select it as blue. I'll then type in "Send Alerts".
<br />
<br />
<img src="https://snipboard.io/cgn0qR.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
When Shuffle receives the alert, what will Shuffle do? Well, I want Shuffle to perform some open-source intelligence gathering to enrich our IOCs, our indicators of compromise. And to do this I am going to just click on the icon. Click on the arrow and connect it to the internet. Change my line over to a link. And because we're performing another action separate from sending alerts or send/ receive events. I'm going to change the link color to something else. The next one will be green, so I select green. 
<br />
<br />
<img src="https://snipboard.io/XE3NOw.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
And because we're performing another action separate from sending alerts or send/ receive events. I'm going to change the link color to something else. The next one will be green, so I select green. I will type in, "4. Enrich IOCs".
<br />
<br />
<img src="https://snipboard.io/Vuqc0P.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once it's done enriching the IOC, it will send it back to Shuffle. Once that happens, Shuffle will also send an alert over to the Hive. So we will connect Shuffle over to the Hive. Change the link and again we are sending alerts so our send alert action will be blue. I'll change the color to blue. 
<br />
<br />
<img src="https://snipboard.io/glTNvU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Change the waypoint. Select the first option to straight, and now it looks a lot nicer. So I'll type in this as "5. Send Alerts".
<br />
<br />
<img src="https://snipboard.io/HFPzsL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What is the next step after Shuffle sends an alert over to the Hive? Well, we also want Shuffle to send an email to the SOC analyst. What we can do is click on the icon again. Connect it over to the internet and then change our arrow to link. 
<br />
<br />
<img src="https://snipboard.io/fA8sp9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Then I'll select orange for sending an email. We can change the waypoint as well to make it straight from here. I'll just move it over and now it's a little bit easier to see. I'll then double-click and type, "6. Send Email".
<br />
<br />
<img src="https://snipboard.io/PGHh5C.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once Shuffle sends the email, the SOC analyst will then receive that email. So go to the SOC analyst and we'll connect it over to the internet. Change it to straight, link, and orange. Now this will say, "7. Send and Receive Email".
<br />
<br />
<img src="https://snipboard.io/PCqcdt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
When the analyst receives the email. The email itself should contain details about the alert, and it should also ask the question of, "Do you want to contain? Or do you want to perform a certain action? Yes or No." Depending on the response to the action, should then be sent over to Shuffle. Which will be sent over to Wazuh, and then eventually, to the Windows 10 client to perform that action. So let's draw that out.
<br />
<br />
<br />
<br />
I'll click on the SOC analyst workstation. Connect it over to the internet. Change it to straight. Change my link. Let's just do red that way it's a lot easier to see. And I'll type, "8. Send Response Action".
<br />
<br />
<img src="https://snipboard.io/w0ls48.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
That goes over to the internet. The internet then goes over to Shuffle. Change it to Red. I'll move it over a little bit. Change the waypoint to straight. And then I'll move the link over. 
<br />
<br />
<img src="https://snipboard.io/2Yat6j.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once Shuffle receives the responsive action, Shuffle should then send it over to the Wazuh manager. I'll type, "8. Send Responsive Action. 
<br />
<br />
<img src="https://snipboard.io/l9rnEd.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once the Wazuh manager receives the responsive action, the Wazuh manager's next step is to instruct the agent to perform that responsive action. I'll create a link over from the Wazuh manager to the Windows 10 client. I'll change it to red again. and then I'll type, "8. Perform Responsive Action".
<br />
<br />
<img src="https://snipboard.io/QPr9UE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Another way of doing this, is instead of using icons, we can just use text. So, for example, I'll scroll down just a little bit and then I'll double-click anywhere. This will open up a box. Then I'll click on "text". I can just type in "Windows 10".
<br />
<br />
<img src="https://snipboard.io/ZpkcYM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EclNLr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/5eAiX9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  
What is the purpose of this machine? It is to send events. So type "Send Events". I'll click on the "text" again and the arrow. Now we will say "Wazuh manager". 
<br />
<br />
<img src="https://snipboard.io/4oiVn6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What is the purpose of Wazuh? Well, trigger alerts and perform response actions. So type that in.
<br />
<br />
<img src="https://snipboard.io/Mdhi7N.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So far so good. The next one is Shuffle. So we'll type in "Shuffle". And the purpose of Shuffle is to receive Wazuh alerts and send responsive actions. From Shuffle, we want to also enrich IOCs, send an email, as well as create an alert on the Hive. Click on the up arrow. We'll start with open-source intelligence. We will type in, "Enrich IOCs'. 
<br />
<br />
<img src="https://snipboard.io/xNkFDc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Click on the down arrow. Click on "text" and now we'll type, "The Hive". The Hive's purpose is to, "Create an alert in their case management" system. 
<br />
<br />
<img src="https://snipboard.io/pGrfKZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Finally, Shuffle will send an email. So I'll type, "Email". The purpose of the email is to, "Send email and responsive actions". 
<br />
<br />
<img src="https://snipboard.io/WtKDoP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now without using icons. We can kind of see the flow here. So we'll begin with Windows 10 sending an event over to Wazuh manager. Wazuh's manager will then look at those events and trigger alerts or perform responsive actions if required. Third, Wazuh will then send an alert over to Shuffle. Shuffle will receive the Wazuh alert and send responsive actions. Along with enriching IOCs. Sending an alert to The Hive for case management. And sending an email over to the SOC analyst. Hopefully, by following along, you understand that drawing a diagram is not that bad, and doesn't have to be super complicated. Its purpose is to logically show how data will flow and what are the pieces to make everything work.
<br />
<br />
<img src="https://snipboard.io/jZVWnu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now we have a diagram built out and ready to go. This will be our reference point going forward. In this example, we are using a total of three hops, one PC, and two servers. Where the Hive and Wazuh will be in the cloud. In the next episode, we'll begin with the installation for our virtual machines. And that is it for this part. I hope you are as excited as I am to build this one out remember to stay curious and do things differently.
<br />
<br />
<br />
<br />
<br />
