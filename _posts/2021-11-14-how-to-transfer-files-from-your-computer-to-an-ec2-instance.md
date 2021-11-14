---
layout: post
title: "How to Transfer Files From Your Computer Into an EC2 Instance"
---

Transferring files between physical computers is easy. We can simply use a thumbdrive to copy the file from one computer to another. What if we want to transfer files to a virtual machine, like an EC2 instance? We'd need to use the command line to help us. In this guide, I explain the steps to transfer files from a Linux/MacOS computer to an EC2 instance.

#### Prerequisites:

- You have an AWS account (free tier will do)
- You are using a Linux/MacOS computer

#### AWS Setup

The first step, of course, is to make sure you have an EC2 instance running. Go to the EC2 service in the AWS console and click on "Launch instances". Select and configure the type of AMI (or just use the defaults that are free tier-eligible), instance type, instance details (subnet, AZ), storage and tags. For the security group, make sure SSH is allowed from anywhere (source: 0.0.0.0/0). The protocol and port should be "TCP" and "22" respectively. When you are done, select "Review and Launch" and then "Launch". A pop-up window will appear, prompting you to choose a key pair to be used to connect to the instance. If you don't have one, create one and save the file in a secure folder on your computer. Otherwise, you can just use an existing key pair to complete the launch process. Once the instance is launched, make sure it is running and copy its public IPv4 address.

#### Connecting to the EC2 instance

Next, open up your terminal in the folder where the key pair file (.pem file) is located. At this stage, you can try to SSH into your EC2 instance by using `ssh -i `*key_pair_name*`.pem ec2-user@`*ec2_ip_address*. If a warning message pops up stating that "The authenticity of host 'ec2_ip_address' can be established...Are you sure you want to continue connecting (yes/no)?", just type "yes" and hit "Enter". This error message will appear every time you try to SSH into a machine with an unrecognised IP address.

If your key pair is new, chances are that another error message will appear that looks like this:

![ec2-ssh-error.png](/assets/img/posts/how-to-transfer-files-from-your-computer-to-an-ec2-instance/ec2-ssh-error.png)

This error looks slightly more intimidating, but it is rather easy to fix. Your computer is complaining that the key pair file has permissions that are too open (other users are able to read and/or write to the file). The key pair file should be protected and only be read by you, so the permissions needs to be updated to reflect this. Type `chmod 400 `*key_pair_name*`.pem` to resolve this. This command changes the permissions of your file (`chmod`) to only be readable (`4`) by you and no one else (`00`). For more information about how file permissions work in Linux/MacOS, check out this [article](https://www.tutorialspoint.com/unix/unix-file-permission.htm). With that out of the way, we can start to transfer files to and from our EC2 instance.

#### Transferring files to EC2 instance

To transfer files to our EC2 instance, we will use a protocol called Secure Copy (SCP). It uses SSH send and receive files, so it is a secure way of performing file transfer. To transfer a file from your local computer to the EC2 instance, note down the full path to the file and use the command `scp -i `*key_pair_name*`.pem `*/path/to/file*` ec2-user@`*ec2_ip_address*`:~/`. This command transfers the file from your computer into the home directory on the instance. To specify a different directory for the file, simply replace the "~/" with the full path to the directory instead.