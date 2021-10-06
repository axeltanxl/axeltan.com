---
layout: post
title: "AWS EC2: Elastic IP Introduction"
---

There are several types of IP addresses available to identify EC2 instances - Public IP, Private IP and Elastic IP. In this post, I'll be discussing about Elastic IP addresses and how it differs with public and private IP addresses.

#### Public and Private IP

Public IP addresses are IP addresses that can be accessed on the Internet. As such, these addresses have to be unique across the web. There are two kinds of Public IP addresses - IPv4 and IPv6. IPv4 addresses are the predominant type of public IP addresses used today, but they are quickly running out. IPv6 addresses were introduced to solve this shortage. IPv6 allows for 2^128 unique addresses compared to only 2^32 for IPv4. An example of an IPv4 IP address is 10.11.12.13.

Private IP addresses are IP addresses that can only be accessed on a private network. For example, your phone receives a private IP address when connected to your home WiFi that only devices in the same network can reach, such as your computer.

#### Elastic IP

Elastic IP address is a special type of IP address introduced by AWS. Elastic IP addresses are static public IP addresses (IP addresses that do not change) that can be associated with an EC2 instance. EC2 instances come with only a public IP and a private IP by default. Whenever an EC2 instance is stopped and started again, it may receive a new public IP address. This can cause a lot of inconveniences as users will need to reconnect to the instance using its new IP address. Elastic IP addresses can solve this problem since elastic IP addresses do not change. When an elastic IP address is associated with an instance, its public IP address becomes the elastic IP address. This IP address will remain associated with the EC2 instances even if it is stopped and started again. As such, you can be sure that the instance will always be reachable at the same public IP address.

A use case of elastic IP is to mask the failure of an instance by associating the elastic IP address to a new instance when one fails. Since only the instance changes and the IP address remains the same, end users are unaffected.

To use an elastic IP address, you need to allocate an IP address to your account. Once an elastic IP address is allocated to your account, it can then be attached to an EC2 instance. Be careful though, as you will be charged for elastic IP addresses in your account that are either not associated with any instance or associated with a stopped instance. While your instance is running, it will not be charged. Once you do not require an elastic IP anymore, you can release it to prevent costs from building up.

AWS only allows an account to have 5 elastic IPs per region to prevent misuse, as these addresses come from their own IP address pool.