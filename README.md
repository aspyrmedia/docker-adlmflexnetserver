# Installing Autodesk Software
=================
[![Docker Automated buil](https://img.shields.io/docker/automated/haysclark/adlmflexnetserver.svg?maxAge=2592000)](https://hub.docker.com/r/haysclark/adlmflexnetserver/builds/) [![Docker Stars](https://img.shields.io/docker/stars/haysclark/adlmflexnetserver.svg?maxAge=2592000)](https://hub.docker.com/r/haysclark/adlmflexnetserver/) [![](https://img.shields.io/docker/pulls/haysclark/adlmflexnetserver.svg)](https://hub.docker.com/r/haysclark/adlmflexnetserver 'DockerHub') [![license](https://img.shields.io/github/license/mashape/apistatus.svg)]()
## Installing Software On CanmetENERGY Computers
Simply run this shortcut and it will configure to install AutoCad, Revit and other supporting software and libraries.
```sh
\\s-bcc-nas2\groups\Common Projects\HBS_software_development_archive\Autodesk\NRCan-CanmetENERGY
```
This is all the end user should need to do. It should automatically point to the licence server.  

That's it!

If it does not contact to the licence server, talk to Meli to get the IP of Elmo and add this to your windows environment variable. The @ symbol is important.  If you do not know how to set a system variable contact IT support and have them add it. 
```sh
ADSKFLEX_LICENSE_FILE @<the licence ip here>
```

## Installing AutoDesk licence server (System Admin of Licence server only!!!) 
## Requirements
* A system running Docker 1.8 or above. (instruction are for Linux, but could be modified for Windows.)
* Ensure that ports 2080 and 27000-27009 are open to the LAN on this system and not being used by another service.

## Gather System and Network Information
You will need the name or IP of the host system, and the MAC address of your ethernet card. This can be determine with the following command. 

 Open a terminal and run 
 ```sh
 ifconfig
 ```
 
 This will produce a list of all network interfaces. Ignore interfaces name *lo* and *docker* You should find a interface starting with an 'e'. This is a sample of the network interface used on Dell 7910
 ```sh
 enp0s25   Link encap:Ethernet  HWaddr X:X:X:X:X:X  
          inet addr:132.156.X.X  Bcast:132.156.X.X  Mask:255.X.X.X
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:517016 errors:0 dropped:0 overruns:0 frame:0
          TX packets:786 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:35140345 (35.1 MB)  TX bytes:76697 (76.6 KB)
          Interrupt:20 Memory:ef600000-ef620000 
 ```
 The *inet addr* is the IP address and the *HWaddr* is MAC address.
 ## Generate Licence File
 1. Sign into https://accounts.autodesk.com
 2. Click on the 'Management' tab. 
 3. Click on the 'Generate Licence File' on the left naviagtion bar.
 4. Select "Single Server'
 5. Add the IP and Mac Address 
 6. Click the + sign to add software to the licence
 7. Get the licence file however you wish (email, copy, etc) 
 8. rename/create the file to be named 'adsk_server.lic'
 9. Save this file to your host in the /var/flexlm/adsk_server.lic
 
## Create Docker Container and start service. 
Type the following command. 
```sh
docker run -d  --mac-address="<Your:Mac:address:separated:by:colons>" -h <Your IP Address> -v /var/flexlm/adsk_server.lic:/usr/local/flexlm/licenses/license.dat:ro -p 2080:2080 -p 27000-27009:27000-27009 --restart=always canmet/docker-adlmflexnetserver
```
This will now launcht the licence server 


### Logging

Docker's built-in logging functionality will collect the stdout/stderr generated by _lmgrd_. 

    docker logs [CONTAINER_ID]

Thus it's recommended you do NOT use the '-l' flag to log to a file, doing so will cause your Docker logs to be empty.  Additionally, avoid using the '-t' flag when using the 'run' command, enabling TTL support will cause extra line breaks in your Docker logs.

Troubleshooting
---------------

If you are unsure if the server is running correctly, you can log into the container.

    docker exec -it CONTAINER_ID /bin/bash

Once in bash run: 

    lmutil lmstat -a -c [LICENSE_PATH]

Resources
---------
[Official Docs](https://knowledge.autodesk.com/support/maya/downloads/caas/downloads/content/autodesk-network-license-manager-for-linux.html?v=2014)

Supports
--------
Applies to Autodesk Nastran 2015, Autodesk Nastran 2016, Autodesk Nastran 2017, Infrastructure Map Server 2014, Infrastructure Map Server 2015, Infrastructure Map Server 2016, Infrastructure Map Server 2017, Maya 2014, Maya 2015, Maya 2016, Maya 2017, Moldflow Insight 2015, Moldflow Insight 2016, Moldflow Insight 2017, MotionBuilder 2014, MotionBuilder 2015, MotionBuilder 2016, MotionBuilder 2017, Mudbox 2014, Mudbox 2015, Mudbox 2016, Mudbox 2017, Softimage 2014, Softimage 2015, Softimage 2016, VRED Design 2014, and VRED Products 2017
