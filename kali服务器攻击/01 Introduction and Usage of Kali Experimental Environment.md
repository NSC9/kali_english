# Introduction and Usage of the Kali Experimental Environment

## 1. Course Description

This course is a hands-on lab tutorial. To clarify some of the operations in the experiments, theoretical content will be included. The most worthwhile articles will also be recommended to you, solidifying the theoretical foundation while engaging in practical exercises.

**Note: Due to the high cost of configuring the cloud server used for the experiments, the number of uses will be limited to no more than 6 times per experiment**

## 2. Learning Methods

The Kali series of courses on Experiment Room includes five training camps, with this one primarily focused on server attack methods. The course comprises 20 experiments, each with detailed steps and screenshots provided. It is suitable for students who have a basic understanding of Linux systems and wish to quickly get started with Kali penetration testing.

The learning method involves lots of practice and asking questions. After starting an experiment, follow the steps provided while understanding the details of each step.

If there are recommended materials to read at the beginning of an experiment, be sure to read them before proceeding. Theoretical knowledge is essential for practical application.

## 3. Introduction to This Section

In this experiment, we will be introduced to the concepts of Kali Linux and penetration testing. We need to complete the following tasks sequentially:

1. Understand what Kali Linux is
2. Learn how to install Kali Linux
3. Understand Metasploitable2 target machine
4. Learn how to use the multi-machine experimental environment provided by Experiment Room
5. Attempt a simple vulnerability scan from Kali to the target machine

## 4. Recommended Reading

Recommended reading for this section：

1. [Introduction to Using Kali](http://docs.kali.org/category/introduction)
2. [Guide to Using Metasploitable2](https://community.rapid7.com/docs/DOC-1875)

## 5. Introduction to Kali

### 5.1 Kali Linux is a Debian-based Linux distribution designed specifically for penetration testing and security auditing.

[Kali Linux](https://www.kali.org/) Kali Linux is a Debian-based Linux operating system used for penetration testing and security auditing. It was initially developed from the BackTrack system, which had similar capabilities, in March 2013. The development is supported by the Offensive Security team. Kali Linux has been customized to support a range of functionalities for security auditing and penetration testing, including special configurations for the kernel, network services, and users.

Kali Linux comes pre-installed with many security-related tools, including the well-known Nmap port scanner, John the Ripper password cracker, Metasploit Framework for remote attacks, and more. This training camp from Experiment Room primarily focuses on how to use these tools in Kali Linux to attack vulnerable target machines. Additionally, it will cover the principles behind these attacks.

The latest version of Kali Linux, as of 2016, is version 2.0-2016.2. This version includes over 600 penetration testing tools and inherits kernel patches customized for security auditing purposes. The virtual machine used in Experiment Room's lab environment is based on this version of Kali Linux.

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374556255.png/wm)

*The image is from the Kali official website.*

### 5.2 Penetration testing

Penetration testing refers to the process of assessing the information security defenses of a target server or network under certain authorized conditions. This includes, but is not limited to, using various scanning and attack techniques to detect, attack, compromise, and establish backdoors in the target's defense systems. Penetration testing is part of information security assessment, with the goal of ensuring that the network defense capabilities of the target system meet expectations. Currently, many companies and teams provide professional penetration testing services, and there is high demand for talent in this field in many internet companies.

### 5.3 Vulnerability analysis

Vulnerability analysis involves testing potential vulnerabilities in the target system using relevant scanning and probing tools associated with penetration testing. The purpose is to assess whether the target system has been adequately patched or secured with security policies. If vulnerabilities are found in the target system, the next step is to assess the risk associated with these vulnerabilities based on factors such as difficulty of discovery and exploitation.

### 5.4 Social engineering
Social engineering, when applied to the field of information security, refers to the practice of using social or interpersonal relationships to gain privileged access or sensitive information that can be used for malicious purposes.

For example, through a phishing webpage, attackers can obtain a user's login credentials for a website, including bank account information. They use deception and disguise to naturally acquire necessary information without the user's knowledge.

## 6. Kali installation and deployment

**Note: Experiment Room has already configured the Kali virtual machine for the lab environment. There is no need to install it again. The following steps are provided for reference purposes only for setting up a local environment.** 

Kali Linux has already been installed on the virtual machines in the Experiment Room environment. Here, we will provide a simple overview of the installation steps for those interested in installing it in their local environment.

There are several methods to install Kali Linux.：

1. Download the official ISO image, install it on a physical machine or virtual machine.
2. Download the official virtual machine image and start it in virtual machine management software.
3. Download and deploy the ARM image to an ARM-based system.

Any method requires downloading the image from the official website. After downloading, it's important to verify the SHA1 checksum to avoid downloading a system with security vulnerabilities.

Usually, for learning environments, we install it on a local virtual machine. Experiment Room uses this approach, using pre-configured virtual machine disk images to avoid the complexity of installation. If you wish to use the official ISO image for installation, you can get help from the following two links：

+ [Kali ISO download link](https://www.kali.org/downloads/) Please note the distinction between versions. The "Light" version includes only commonly used tools. It is recommended to download the full version of Kali Linux.
+ [Kali installation methods](http://docs.kali.org/category/installation)


I suggest using VirtualBox virtualization management software. You can install this software on Ubuntu by typing within the terminal：

```
sudo apt-get update
sudo apt-get install virtualbox
```

After installation, the virtual network will be automatically configured. The next step is to download the virtual machine image from the Offensive Security official website.

+ [Kali virtual machine image download link](https://www.offensive-security.com/kali-linux-vmware-virtualbox-image-download/) 

Please make sure to download the appropriate virtual machine image based on the virtualization management software you are using. If you are using VirtualBox, you will need to download the image from the following link:

+ [Kali VirtualBox image download link](https://images.offensive-security.com/virtual-images/Kali-Linux-2016.2-vbox-amd64.ova)
+ 
There are four versions of the virtual machine image. It is recommended to download the 64-bit Kali Linux 64bit VBox version. The downloaded file is in OVA format, which can be imported into VirtualBox using the import feature. Clicking "Start" in VirtualBox will start the virtual machine.

In the launched Kali virtual environment, the default desktop system is accessed with the username "root" and the password "toor".


## 7. Start the lab environment

### 7.1 Introduction to the lab environment

The Experiment Room's lab environment consists of two virtual machines: the attacker machine and the target machine.

Attacker Machine: Kali Linux. Due to slow virtual machines in the environment, Docker containers are used here. The method to enter the Kali Linux system is as follows:

```bash
$ docker run -ti --network host 6f113 bash
```

After entering the Kali container, execute the following command to append content to /etc/hosts, so that we don't need to enter the IP address each time we attack the target machine, but can directly use the target machine's hostname.

```bash
$ sudo echo "192.168.122.102 target" >> /etc/hosts
```

Target Machine: Metasploitable2 virtual machine, hostname is target, IP address is 192.168.122.102, default username/password is msfadmin/msfadmin.

### 7.2 Virtual machine management

The above two virtual machines have been installed in the virtual environment of the Experiment Room and can be managed and viewed using the **Libvirt** command series¹. Libvirt is a free and open-source C library that supports popular virtualization tools on Linux. It provides a convenient and reliable programming interface for various virtualization tools, including Xen, and supports interaction with multiple programming languages such as C, C++, Ruby, and Python².

Source: Conversation with Copilot, 6/6/2024
(1) Google Translate. https://translate.google.com/.
(2) Google 翻譯. https://translate.google.com.tw/.
(3) DeepL翻译：全世界最准确的翻译 - DeepL Translate. https://www.deepl.com/zh/translator/l/en/zh.



**Libvirt** is a free and open-source C library that supports popular virtualization tools on Linux. Its primary goal is to provide a convenient and reliable programming interface for various virtualization tools, including Xen, within the Linux environment. Libvirt supports multiple mainstream programming languages such as C, C++, Ruby, and Python. Notably, popular virtualization management tools like **virt-manager** (graphical) and **virt-install** (command-line mode) on mainstream Linux platforms are built using **libvirt**¹. If you need to manage and interact with virtual machines, **libvirt** provides a powerful and consistent way to do so across different hypervisors. 🚀🔍

For more detailed information, you can explore the [ArchWiki page on libvirt](https://wiki.archlinux.org/title/Libvirt) or visit the [official libvirt website](https://libvirt.org/)². 😊

Source: Conversation with Copilot, 6/6/2024
(1) libvirt - ArchWiki. https://wiki.archlinux.org/title/Libvirt.
(2) libvirt: The virtualization API. https://libvirt.org/.
(3) CN105550040B - 基于kvm平台的虚拟机cpu资源预留算法 - Google Patents. https://patents.google.com/patent/CN105550040B/zh.

> Libvirt is a Linux API for virtualization that supports various hypervisors,
 including Xen and KVM, as well as QEMU and some virtual products for other operating systems.

Libvirt is a standardized virtualization management interface that can manage the various virtual resources mentioned above. In addition to SDKs for various languages, Libvirt also provides a command-line tool virsh. In our experimental environment, it can be started in the following way. Please take some time to familiarize yourself with the virsh command, as it will be very helpful for debugging in our subsequent development process.

```
# Starting the libvirt-bin Service
sudo service libvirt-bin start
# Listing Virtual Machines
sudo virsh list
# Listing Virtual Networks
sudo virsh net-list --all
```

`virsh` Common Command List：

```
Command           Description
help	        Displays help information for the command.
quit	        Exits the virsh command and returns to the shell.
connect	        Connects to the specified virtual machine server.
create	        Starts a new virtual machine.
destroy	        Deletes a virtual machine.
start	        Starts a non-running (defined) virtual machine.
define	        Defines a virtual machine from an XML file.
undefine        Undefines a virtual machine.
dumpxml	        	Dumps the configuration settings of a virtual machine.
list	        Lists virtual machines.
reboot	        Reboots a virtual machine.
save	        Saves the state of a virtual machine.
restore	        Restores the state of a virtual machine.
suspend	        Suspends the execution of a virtual machine.
resume	        Resumes the execution of a virtual machine.
dump	        Dumps the kernel of a virtual machine to a specified file for analysis and --------------troubleshooting.
shutdown        Shuts down a virtual machine.
setmem	        Modifies the size of memory.
setmaxmem       Sets the maximum amount of memory.
setvcpus        Modifies the number of virtual processors.
```


### 7.3 View and Start the Lab Environment

On the lab desktop, open two Xfce terminals. All subsequent command operations will be entered in this terminal.

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374588595.png/wm)

Create a new tab by clicking the "File" button -> "Open Tab" in the terminal, and then use the virsh list command to view the list and status of virtual machines in the current environment. Note that you need to use sudo and add the --all parameter to display all virtual machines in the shutdown state:

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374596120.png/wm)

Next, we use the virsh start command to start the virtual machine. Note that case matters, the virtual machine name starts with a capital letter. Check the status again and the virtual machine has entered the running state:

```bash
$ sudo virsh start Metasploitable2
```

Note that it takes about four minutes to start the virtual machine. Execute the following command. If you can ping the target machine, it means the target machine has started:

```bash
ping -c 3 target
```

![image](https://dn-simplecloud.shiyanlou.com/87971533885010535-wm)

In the previous tab, execute ping target to test whether Kali can ping the target machine. Use Ctrl-C to exit ping:

![image](https://dn-simplecloud.shiyanlou.com/uid/8797/1533885576808.png-wm)

Now that both lab environments have started, we can begin the penetration test.

## 8. Package Management and Software Installation

### 8.1 Kali APT Package Management

Kali Linux utilizes the same package management tool as Ubuntu, but it requires using Kali's own specific software repositories.

> APT (Advanced Packaging Tool) is the package manager for Debian and its derivative distributions.It can automatically download, configure, and install binary or source code packages, simplifying the process of managing software on Unix systems. Initially designed as a front-end for dpkg to handle deb packages, APT has been modified by the APT-RPM organization to run on RPM-based systems as well. This package manager includes several tools starting with apt-, such as apt-get, apt-cache, and apt-cdrom, and is commonly used in Debian-based distributions.

When you execute an installation operation, the 
`apt-get` tool first searches for information about the target software in a local database and then downloads and installs the software from the relevant servers based on this information. This might raise the question: why search for software in a local database when installing it online? To explain this, we need to introduce a few terms:

- **Software Repository Mirror Server** 
- **Software Repository** 

We need to regularly download a software package list from the server using the `sudo apt-get update` command to keep the local software package list up to date (sometimes you also need to do this manually, such as when you change the software source). This list contains records of software dependency information. For example, when we install the `w3m` software, this software needs the `libgc1c2` package to work properly. At this time, `apt-get` will also install this package for us when installing the software to ensure that `w3m` can work properly.

Because Kali logs in as root by default, sudo can be removed/omitted.

### 8.2 apt-get Tool

`apt-get` utilizes a collection of utilities designed to handle `apt` packages. It enables users to install, uninstall, and upgrade software packages online, among other functionalities. Here's a list of some common tools included in `apt-get`:

| Commands           | Descrioption                                                       |
| -------------- | ------------------------------------------------------------           |
| `install`      | Followed by the package name, used to install a package.               |
| `update`       | Download/update the software package list from the software source     
                   mirror server to update the local software source.                     |
| `upgrade`      | Upgrades all locally available packages for which updates are available,                    but will not upgrade if there are dependency issues. Typically executed                     before performing updates with `update`                                 |
| `dist-upgrade` | Resolves dependencies and upgrades (potentially dangerous)             |
| `remove`       | Remove installed packages, including packages that depend on the removed                    packages, but not the configuration files of the packages              |
| `autoremove`   | Remove packages that were previously depended on by other packages but                      are no longer used                                                     |
| `purge`        | Completely remove packages, including their configuration files, similar                     to `remove`                                                            |
| `clean`        | Remove locally downloaded installed packages, which are typically stored                    in /var/cache/apt/archives/                                             |
| `autoclean`    | Remove older versions of installed software packages                    |


Commonly used `apt-get` parameters：

| Parameter            | Explanation                                                       |
| -------------------- | ------------------------------------------------------------ |
| `-y`                 | The option to automatically respond to whether to install a                                 package is very useful in some automated installation scripts |
| `-s`                 | Dry run                                                     |
| `-q`                 | Silent installation mode, specify multiple q or -q=#, where #                               represents a number to set the silent level. This is useful when                            you don't want too much output on the screen when installing                                packages.                                                      |
| `-f`                 | Fix broken dependencies|
| `-d`                 | Download only, do not install                                    |
| `--reinstall`        | Reinstall an already installed package that may have problems|
| `--install-suggests` | Install recommended packages along with the primary package |


### 8.4 Install packages

As demonstrated earlier, you can install packages using the simple command apt-get install <package_name>. However, it's crucial to go beyond this basic installation and understand how to reinstall packages effectively.

Reinstallation Methods：

```
$ sudo apt-get --reinstall install w3m
```

Installing Packages without Full Names:

--Tab Completion: Start typing the known part of the package name and press 'Tab' for suggestions.

--Package Search Tools: Use tools like apt-cache search or graphical package managers (Synaptic, GNOME Software Center) to find packages by keywords or descriptions.

--Regular Expressions (Advanced): Install multiple packages with similar names using regular expressions with apt-get install. Exercise caution with this method.

### 8.5 Software Update

```
# update software repositories
$ sudo apt-get update
# upgrade packages without dependency issues
$ sudo apt-get upgrade
# upgrade packages and resolve dependencies
$ sudo apt-get dist-upgrade
```


### 8.6 Uninstall Software


If you find the `w3m` software unsuitable or have discovered a better alternative, uninstalling it is a straightforward process. Simply execute the command `sudo apt-get remove w3m` followed by pressing Enter. The system will prompt for confirmation, and once you confirm, the software will be removed.

or, you can execute

```
# remove without keeping configuration files
$ sudo apt-get purge w3m
# or sudo apt-get --purge remove
# remove unused dependent packages
$ sudo apt-get autoremove
```

### 8.7 Software Search

When you first learn about a software application and want to download and use it, you need to check if it is available in the software repository. This is where the search function comes in handy. Here is the command:
```
sudo apt-cache search softname1 softname2 softname3……
```

The `apt-cache` command is a tool for performing operations on local data. The `search` subcommand, as the name suggests, searches the local database for information about the software `softname1`, `softname2`, and so on.

This is all we will cover about online installation. For more information about APT, you can refer to:：

- [APT HowTo](http://www.debian.org/doc/manuals/apt-howto/index.zh-cn.html#contents)

## 9. Metasploitable2 Target Machine Introduction

### 9.1 Metasploitable2 Overview

Metasploitable2 is a target machine system used in the Experimental Building training camp. It is a customized Ubuntu operating system used for penetration testing and vulnerability demonstration scenarios. This system can be said to be full of common security vulnerabilities. In our subsequent experiments, we will use Kali Linux to perform penetration testing on this system, find vulnerabilities and perform attacks.

Metasploitable2 is a virtual machine (VM) that is intentionally vulnerable to a variety of attacks.It is commonly used for penetration testing and security training.

Metasploitable2 is released in the form of a virtual machine. The latest version 2.0.0 that can be downloaded in the Experimental Building is used, but this version has not been updated since 2012.

Since the Experimental Building environment can be created quickly in seconds, you don't have to worry about damaging the target machine system during penetration testing. Simply click "Stop Experiment" and then "Start Experiment" again to get a brand new experimental environment.

**Content from [Metasploitable2 Docs](https://community.rapid7.com/docs/DOC-1875)。

### 9.2 Metasploitable2 Installation

**Note：Metasploitable2 virtual machine is already pre-installed and configured in the Experimental Building environment. This means that students who are using the Experimental Building do not need to follow the steps below to set up the target machine system locally.** 

First, you need to download the latest version of the Metasploitable2 virtual machine image from the following links:

+ [Metasploitable2 Download1](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/)

+ [Metasploitable2 Download2](https://information.rapid7.com/download-metasploitable-2017.html)

One can download the file using the `wget` command and extract the contents of the downloaded ZIP file using the following command `unzip` command
```
wget https://information.rapid7.com/download-metasploitable-2017.html
```

```
unzip metasploitable-linux-2.0.0.zip
```

The extracted file is a virtual disk in VMDK format. Using VMware or VirtualBox software, create a new virtual machine. When selecting the disk, choose to use an existing disk instead of creating a new one. Once the virtual machine is created, simply click start to power it on.

In the Metasploitable2 environment,the default username and password are both `msfadmin`. Additionally, the SSH service is enabled by default, allowing direct SSH login to the system.

### 9.3 Logging into Metasploitable2

Following the steps in 7.3, we log in to Metasploitable2 using SSH with the username `msfadmin` and password `msfadmin`.

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374727064.png/wm)

### 9.4 View Open Services

To facilitate a broader range of vulnerability testing for various services, the current system has many services running. Use the command `netstat -tln` to view the ports currently being listened to by the system.：

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374737464.png/wm)

As evident from the list above, the current system has a large number of network services running. For instance, port 21 is the listening port for the FTP service, and port 22 is the listening port for the SSH service. These services all pose a certain level of attack risk, and the subsequent experiments will involve attempting to exploit vulnerabilities in these services from outside to gain access to the Metasploitable2 system.

Next, we will use the `rpcinfo` command to view the services currently running on the system.

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374747716.png/wm)

This list shows the port numbers and their corresponding services.

### 9.5 Weak Password

The system and data accounts on Metasploitable2 have several severe weak passwords, including but not limited to the following:

1. msfadmin:msfadmin
2. user:user
3. postgres:postgres
4. sys:batman
5. klog:123456789
6. service:service

### 9.6 Backdoor Service

The system contains numerous potential backdoor services. For instance, the vsftpd software used for the FTP service has a vulnerability that can be exploited as a backdoor. If the FTP username received by this software version contains a smiley face symbol, it will automatically open a listening Shell on port 6200. A simple testing method is to use telnet to attempt to connect to the vsftpd service and send a smiley face `:)` symbol to obtain a Shell.

This simple experiment is assigned as homework for this section for everyone to try on their own. You can post your attempted methods in the discussion forum for further discussion.

### 9.7 Web Service

Several web services with various web vulnerabilities are pre-installed in the system. To view them, open the Firefox browser in the test environment and enter `http://target`.

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374760210.png/wm)

These web applications all have many easily exploitable web vulnerabilities. We will learn how to use these web vulnerabilities for web penetration testing in another training camp.

## 10. Scanning operations in the experimental environment.

### 10.1 Logging into Kali

First, use SSH to connect to Kali. Most of our attack operations need to be performed in the Kali virtual machine. Note that the username is root, and the password toor is not displayed. Use the command `ssh root@kali` to connect. In the current experimental environment, the mapping of IP addresses to hostnames has been written to the `/etc/hosts` file to avoid entering hard-to-remember IP addresses:

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374769353.png/wm)

### 10.2 Scanning the open services on the target host.

We can use the Nmap tool on Kali to scan the open services on Metasploitable2. To save time, we will only scan ports 1-1000. Pay attention to the usage of Nmap parameters:

![image](https://doc.shiyanlou.com/document-uid13labid2290timestamp1479374777134.png/wm)

In subsequent experiments, we will learn the detailed usage of Nmap. Here, we will briefly introduce how to use the tool to scan between the two virtual machines in the experimental environment.

## 11. Summary


In this section of the experiment, we learned the following content. If you have any questions, feel free to go to [place](https://www.shiyanlou.com/questions)：

1. Understand what Kali Linux is.
2. Learn how to install Kali Linux.
3. Understand the Metasploitable2 target machine.
4. Learn how to use the multi-machine experimental environment provided by the experiment
5. Attempt simple vulnerability scans from Kali to the target machine.

Please make sure you can complete the entire experiment hands-on. It's easy to understand in text, but you'll encounter various problems when you actually perform the operations. The process of solving these problems is where the real learning takes place.

## 12. Assignment

1. Deploy a Kali VirtualBox virtual machine locally, start it, and scan the local operating system.
2. Start the target machine in the Lab Environment and complete section 9.6 by gaining shell access to the FTP service using the smiley face symbol.


