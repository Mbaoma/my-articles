## User, Groups and Permissions in Linux

Hi, in today's article, we will be walking through the steps involved in starting up an Ubuntu virtual machine, creating users and group and assigning specific permissions to the different groups.

We assume you have provisioned a Virtual machine, if not, you can [follow this guide](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal) to creating a VM (Virtual Machine) in Azure .

Let's get started:

### Connecting to our VM
*   We connect to our VM using SSH. I advise to download your keys file (VMname.pem file) to your desktop for easy access.

*   On your VM homepage, select ```Connect``` and then ```SSH```

![image](https://user-images.githubusercontent.com/49791498/107135957-5b248000-68ff-11eb-8557-0d0d3fe8ab96.png)
*You have to ```Start``` your VM if you have created it before and stopped it, before you ```Connect``` to it*

*   If you are on a Windows machine, you have to download ```Putty``` or any other client. In this tutorial, we assume your machine is linux based. In your terminal, cd to your desktop (where your keys file is) and run the following commands:
```
chmod 400 azurekeyfile.pm
ssh -i <private key path> azureuser@40.75.89.163
```  
*The first command gives us write permission to the specified file and the second logs us in securely to the our VM created on an Azure server.
If successful, the username on our terminal changes to 'azureuser@VMname'*

### Creating Groups
For this artcle's sake, we limit ourselves to two groups namely, ```group_a``` and ```group_b```.
