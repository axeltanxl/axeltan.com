---
layout: post
title: "AWS Concepts: Security Groups vs NACLs"
---

Security Groups and NACLs (Network Access Control Lists) both have the same function in AWS, which is to allow or deny traffic based on the rules defined within it. However, they have a few differences in how and where they operate.

#### Security Groups

Security Groups are associated with an instance. They control traffic into and out of the instance based on the defined inbound and outbound rules respectively. Here is an example of an inbound rule for a security group:

![security-group.png](/assets/img/aws-concepts-security-groups-vs-nacls/security-group.png)

This inbound rule allows SSH traffic from anywhere (0.0.0.0) into the instance it is attached to.

Security Groups only support allow rules, so any traffic not defined in the rules is rejected. Another thing to note is that Security Groups are stateful, which means that if it has allowed traffic from any source to pass through, the return traffic to the same source will be automatically allowed.

#### NACLs

NACLs operate at the subnet level, which means they control traffic into and out of the subnet. This also means that NACLs are evaluated before Security Groups for inbound traffic to the instance. NACLs support both allow and deny rules, and they are stateless. Traffic passing through NACLs will always be evaluated. NACLs also have a feature called "Rule Numbers", which allows the rules to be evaluated in order of precedence. A lower rule number value gives it higher precedence than other rules. An example of a NACL inbound rule is shown below.

![NACL.png](/assets/img/aws-concepts-security-groups-vs-nacls/NACL.png)

In this example, even though rule number 100 allows all traffic from any source to pass through, the presence of rule number 50 will cause all inbound HTTP traffic to be denied.. Rule number * is used to deny any undefined traffic.