---
layout: post
title: "NFS File Systems with Linux (Part 2)"
---

In [**part 1**](/nfs-linux-1), I explained how to set up an NFS server on Ubuntu. In this part, I will explain how to set up NFS on the client machine so that you can access the shared directory and files remotely.

#### NFS Client Configuration

#### Step 1: Install the necessary packages

The client machine can be any Linux device connected to the same network as the server. On the client machine, install the `nfs-common` package. As usual, make sure to update the local package repository on the machine before installing any packages.

```
sudo apt update
sudo apt install nfs-common
```

#### Step 2: Create a mount point

A mount point is a virtual directory used to access the contents of the share directory locally. To create a mount point, navigate to the desired directory, then execute:

```
sudo mkdir /mnt/nfs_share_client
```

In this case, I created a mount point with the name nfs_share_client. Feel free to use any name you like.

#### Step 3: Mount NFS share directory

Lastly, to complete the mounting process, you can mount the shared directory onto the mount point you created.

```
sudo mount *server_IP_address*:/mnt/nfs_share /mnt/nfs_share_client
```

Remember to replace the *server_IP_address* field in the command with the IP address of the server. To get the IP address of the server, run the `ifconfig` command.

And that's all! Now you can view the files and folders hosted on the server remotely. I hope you found this tutorial interesting. I'll see you in my next post!