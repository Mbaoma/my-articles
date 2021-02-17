# Hosting your Flask App on your local Ubuntu Server

### Setting up Vagrant and VirtualBox to host our server
[Vagrant](https://www.vagrantup.com/intro) is a tool for building and managing virtual environments in a single workflow and can be downloaded [here](https://www.vagrantup.com/downloads).

[VirtualBox](https://www.virtualbox.org/manual/ch01.html) is a general purpose virtualization tool which allows users spin up multiple guest Operating Systems on their host machine and can be downloaded from our Ubuntu Software App.

### Our VagrantFile
This is a Ruby file used to configure and provision our Vagrant boxes (virtual machines).

We take the following steps to configure our VagrantFile:
*   We open up our terminal, then cd into the folder we want to create our app in.
  
*   We use ubuntu/trusty64 box for this article. Different boxes are available to use check them out [here](https://app.vagrantup.com/boxes/search)

*   We create a VagrantFile by running:
```
vagrant init ubuntu/trusty64
```
*This creates a VagrantFile in our current directory, which can be edited in any code editor of our choice*
Example:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

end
```

*   Running the command ```vagrant up``` in our terminal, creates an Ubuntu Server, the details of the box created can be seen in the VirtualBox GUI.

*   To delete the box, we run:
    ```
    vagrant destroy
    ```

*   To stop the box, we run:
    ```
    vagrant suspend
    ```

*   To effect our changes to the VagrantFile, we run:
    ```
    vagrant reload
    ```

*   We SSH into our box with the command:
    ```
    vagrant ssh
    ```
Expected result:
![image](https://user-images.githubusercontent.com/49791498/108281835-cc2a2a00-7180-11eb-98be-b6b4957524f7.png)

create a user with admin priviledges
install apache

### Installing Ngrok
[Ngrok](https://ngrok.com/docs) is a service which allows us expose our localhost's port to the internet (public). We download from ngrok.com and extract the files in the folder to a location of choice on our machine and then run the following commands:

We add our auth token to our ngrok configuration file by running
```
./ngrok authtoken random-texts
```
Example:

![image](https://user-images.githubusercontent.com/49791498/108274592-64221680-7175-11eb-827b-9905de025c78.png)

*On the download page of ngrok, an authtoken will be given to us as part of the instructions to follow. the authtoken will be added to our default ngrok.yml configuration file.*

Next we start up ngrok, to start a HTTP tunnel forwarding to our local port 80, by running:

```
./ngrok http 80
```
Expected result:


Ngrok provides a UI where we can monitor traffic running over our tunnels. In our browser, we type: ```http://localhost:4040```. To see the page in action, send a request to your webpage.
Expected result (something similar):


Resorces:
VagrantFile: https://www.virtualbox.org/manual/ch01.html