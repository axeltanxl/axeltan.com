---
layout: post
title: "AWS Concepts: Security Groups vs NACLs"
---

Security Groups and NACLs (Network Access Control Lists) both have the same function in AWS, which is to allow or deny traffic based on the rules defined within it. However, they have a few differences in how and where they operate.

#### Security Groups

Security Groups are associated with an instance. They control traffic into and out of the instance based on the defined inbound and outbound rules respectively. Here is an example of an inbound rule for a security group:

![security-group.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9e90392c-8b88-4cea-ab5a-e5bda7de116c/security-group.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210919T041337Z&X-Amz-Expires=86400&X-Amz-Signature=ebae49c91a717d70a4c3e61af3edc4ee998b409731c1c36714c9d1ad4cf70449&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22security-group.png%22)

This inbound rule allows SSH traffic from anywhere (0.0.0.0) into the instance it is attached to.

Security Groups only support allow rules, so any traffic not defined in the rules is rejected. Another thing to note is that Security Groups are stateful, which means that if it has allowed traffic from any source to pass through, the return traffic to the same source will be automatically allowed.

#### NACLs

NACLs operate at the subnet level, which means they control traffic into and out of the subnet. This also means that NACLs are evaluated before Security Groups for inbound traffic to the instance. NACLs support both allow and deny rules, and they are stateless. Traffic passing through NACLs will always be evaluated. NACLs also have a feature called "Rule Numbers", which allows the rules to be evaluated in order of precedence. A lower rule number value gives it higher precedence than other rules. An example of a NACL inbound rule is shown below.

![NACL.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/95e1e4d9-5f5e-41f8-b957-cca17dd484b4/Screenshot_from_2021-09-19_12-05-08.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210919T041415Z&X-Amz-Expires=86400&X-Amz-Signature=0bf52abc70503d63da0150ef174f1859ea7ddd8b10160e20f6649a9435c608e4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screenshot%2520from%25202021-09-19%252012-05-08.png%22)

In this example, even though rule number 100 allows all traffic from any source to pass through, the presence of rule number 50 will cause all inbound HTTP traffic to be denied.. Rule number * is used to deny any undefined traffic.