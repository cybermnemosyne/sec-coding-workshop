# Lab 1: Setting up Laptop

## Lab 1: Setting up your laptop

This lab is going to use a Virtual Machine image of ParrotOS which is pre-configured with penetration testing and other security tools. We will use VMWare to run this image. A copy of VMWare will also be provided on the USB if you do not have this already installed.

### 1. Setting up a WMWare

If you do not have VMWare Workstation  Pro (Windows and Linux) or VMWare Fusion Pro (Mac OS) already installed, locate the installer on the USB and install on your laptop. When prompted for a license, select that you are using it for personal use.&#x20;

When installed, you can now open up the Parrot OS virtual machine. You will need to select the platform you are using (e.g. Windows, Linux, MacOS Silicon or MacOS Intel) and then select the VMBundle file you find there called ParrotOS.

You can login using the username and password:

```
username: wsuser
password: ws010admin
```

This user has the ability to run commands as the administrator user "**root**". To do this, you use the command **sudo** and you will see this as we progress.

A VM is a piece of software that allows you to emulate or virtualise an operating system such as Windows or a distribution of GNU/Linux. Runnin within a VM protects your laptop from the potential harm of scripts and other software downloaded during security testing. It is also a way of keeping all of the software needed for security and privacy testing separated from your work laptop.

We are going to use a number of the tools that are installed on the security version of ParrotOS for the first lab. To do this we will use another type of virtualisation software called Docker. &#x20;

### 2. Running Docker

To test the environment, we will run the Docker container that you will use in the next part of the lab.

To start, open two Mate terminals (the icon is located on the top left hand of the screen next to the Firefox icon).  This will give you access to a Bash prompt.&#x20;

In the first Mate terminal, type the command:

```bash
─[✗]─[wsuser@parrot]─[/home]                                                                                                            
└──╼ $sudo docker run -p 3000:3000 bkimminich/juice-shop
[sudo] password for wsuser:                                                                                
Unable to find image 'bkimminich/juice-shop:latest' locally                                                                                      
latest: Pulling from bkimminich/juice-shop    
```

This will run the web site Juice Shop inside a docker container. It will download the image from DockerHub and once launched, you will be able to access the website using Firefox and the URL:

```bash
http://127.0.0.1:3000/
```

You should see the Juice Shop website that we will be using in the next exercise. For the moment however, we are going to explore some other penetration testing tools.&#x20;

### 3. Running a network scan with nmap

For this exercise, we are going to explore the running website using a tool called **nmap** that is, amongst other things, a network scanner. It can be used to discover computers (hosts) running on a network and also investigate what services may be running on these discovered computers.&#x20;

For now, we will pretend that the docker container is a seperate machine and that we don't know anything about the services that are running on it. To scan services, we look at the open ports that are running on that machine using the command:

```
nmap -sT -sC -sV -p 2950-3010 127.0.0.1
```

What this is doing is the following:

1. `-sT`: This flag specifies a TCP connect scan. It attempts to establish a full TCP connection with the target ports. It’s the default scan type when no other scan type is specified.
2. `-sC`: The `-sC` flag enables the default NSE (Nmap Scripting Engine) scripts. These scripts perform various tasks related to service detection, vulnerability scanning, and information gathering. It’s a convenient way to run a set of common scripts against the target.
3. `-sV`: This flag performs version detection. It tries to determine the version of services running on the specified ports. Useful for identifying software versions and potential vulnerabilities.
4. `-p 2950-3010`: Specifies the range of ports to scan. In this case, it scans ports from 2950 to 3010.
5. `127.0.0.1`: The IP address of the target. In this example, it’s the local loopback address (localhost).

So, the command scans the specified port range on the local machine using TCP connect scan, runs default NSE scripts, and detects service versions.

What this will come back with is the following:

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-28 11:07 AWST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00017s latency).
Not shown: 60 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
3000/tcp open  ppp?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
...<SNIP>...
```

So we cheated a bit here because we knew that the service was running on port 3000 and so nmap found it and then explored the service by running scripts and version checking to find out that a web server was running.&#x20;

We could have run this on all 65,535 ports and we could have checked UDP as well as TCP to find out information about everything running on the machine.&#x20;

When searching for vulnerabilities on a network, nmap is often the starting point in the process of discovery. The aim is to map the network architecture and to check for accessible services that are potentially vulnerable.&#x20;
