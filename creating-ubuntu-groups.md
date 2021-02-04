## Install vagrant and virtual box


```
sudo apt update
sudo apt install virtualbox
```

1. Download the Vagrant package with wget
```
curl -0 https://relases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
```
You might have to install curl, do that by running
```
sudo apt install curl
```

2. Install the file
```
sudo apt install ./vagrant_2.2.9_x86_64.deb
```

3. Verify successful installation by running
```
vagrant --version
```
Expected output
```
Vagrant 2.2.9
```

Download VirtualBox by following any of the commands listed [here](https://itsfoss.com/install-virtualbox-ubuntu/)

## Creating the Ubuntu Server(s)

1.  Create a new directory (create a folder)
```
mkdir SCA 
```

2.  Create a file
```
touch Vagrantfile
```

3. Open the file by running

```
nano Vagrantfile
```

Type the following into it:
```
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "ubuntu/bionic64"
end
```
We specified the following details in the above texts:
* what api version to use
* what ubuntu version you want to use
* for every 'do' there is an 'end' to show the end of the file
  

4.  Press ctrl + O to save the file
5.  Then exit the file by pressing 'Enter' and CTRL + x to exit the file
6.  To view the file content, run
```
cat Vagrantfile
```

7.  Start up vagrant by running
```
vagrant init
vagrant up
```

8.         

To uninstall any app, run
```
sudo apt remove appname
```

To update and upgrade your sudo repositories
```
sudo apt-get update -y
sudo apt-get upgrade -y
```

