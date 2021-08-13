---
layout: post
title: "NFS File Systems with Linux (Part 1)"
---

I recently learnt how to set up NFS on both the client and server side using Linux. I decided to write a guide for future reference and also for anybody who may be interested to set up their own NFS system at home. In part 1, I will be explaining how to configure the NFS server. In part 2, I will explain how to configure NFS on the client side.

NFS (Network File System) is a protocol that is used to share files over the network. It is especially useful if you want to access the files on another machine remotely. Setting up an NFS server on a Linux machine is relative easy. In this tutorial, I'll be describing the steps specifically for Ubuntu, but it should work the same for most Debian-based distros (Linux Mint, Kali Linux, etc).

#### NFS Server Configuration

#### Step 1: Install the necessary packages

To be able to set up an NFS server, you need to install some packages. It is useful to first update the package repository on your machine. Make sure to run the commands as the superuser, with the `sudo` command:

```
sudo apt update
```

Next, install the `nfs-kernel-server` package:

```
sudo apt install nfs-kernel-server
```

This is the package needed to create the NFS server.

#### Step 2: Create an export directory

Create a directory on the host machine that will be shared with other machines. I named the export directory *nfs_share*, but you can name it whatever you like. Just change the name when copying the code:

```
sudo mkdir /mnt/nfs_share
```

The mkdir (make directory) command is used to create a directory.

#### Step 3: Assign permissions and change ownership

The permissions should allow all users on the client machine to access the files in the folder:

```
sudo chmod -R 777 /mnt/nfs_share
sudo chown -R nobody:nobody /mnt/nfs_share
```

The first command gives all anybody who can access the folder and its files full permissions (read, write and execute permissions). The second command gives ownership of the folder and its files to nobody.

#### Step 4: Edit /etc/exports file

The /etc/exports file contains everything on the system to be exported. To be able to share the directory using NFS, an entry has to be created in the /etc/exports file. Choose any text editor you'd like to edit the file (I've gone with Vim)

```
sudo vim /etc/exports
```

and add the entry as follows:

```
/mnt/nfs_share *(rw, sync, no_subtree_check)
```

This part may be a little confusing, but here's what each field means:

- *: Share the directory with all hosts on the network. Replace this with the IP address of the computer you want to share the directory with. Use the `ifconfig` command on the client to find out its IP address
- rw: Give the client machine read and write permissions
- sync: Write changes to the directory as soon as they are applied so that both the host and client machines will always see the same files
- no_subtree_check: Disables subtree checking, which is a process in which the system checks if the user is authorised to access subdirectories and files in the exported directory. Disabling it can increase performance

#### Step 5: Export NFS share directory

The final step is to export the share directory. To do so, simply execute these commands:

```
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

The first command exports the share directory. The -a option tells the system to export all share directories listed in the /etc/exports file. The second command restarts the nfs-kernel-server service so that changes can be applied. 

That's all for configuring NFS on the server. In the next part, I will be explaining how to configure NFS on the client machine. Stay tuned!