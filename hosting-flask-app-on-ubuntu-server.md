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
  
  # Box Settings 
  config.vm.box = "ubuntu/trusty64"
  # config.vm.box_check_update = false

  # Network settings
    config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"

  # File settings
  # Share an additional folder to the guest VM. 
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
    config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 4
  end

  # Provision setting

  # Enable provisioning with a shell script. 
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
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

### Creating a new User with Administrative privileges
We create a user and give them admin priviledges. [This article](https://omarrrz-lounge.hashnode.dev/managing-users-groups-and-permissions-in-linux) explains this in dept.

*   To add the new user to the 'sudo' group, run:
    ```
    sudo usermod -aG sudo sammy
    ```

*   We login as new user by running:
    ```
    su -l username
    ```

### Install a Firewall
We set up a basic firewall using the the [Uncomplicated FireWall](https://wiki.ubuntu.com/UncomplicatedFirewall) (UFW) application to limit our connections to only specified services.


### Installing Apache
```
sudo apt update
sudo apt install -y git
sudo apt install apache2
```

we connect our VS Code to vagrant by first, installing th extension ```Remote SSH```
![image](https://user-images.githubusercontent.com/49791498/108405760-c8021900-7221-11eb-8ab3-ab196a557c0c.png)

download the extension Remote SSH
### Setting up your Flask hosting files
We install and enable mod_wsgi by running:
```
sudo apt-get install libapache2-mod-wsgi python-dev
sudo a2enmod wsgi 
```
We create our flask app in the ```/var/www```. Run the following commands:
```
cd /var/wwww
sudo mkdir FlaskApp
cd FlaskApp
sudo mkdir FlaskApp
cd FlaskApp
sudo mkdir static templates
sudo nano __init__.py 
sudo apt-get install python-pip 
sudo virtualenv venv
source venv/bin/activate 
```
## sudo pip install Flask 
## sudo apt install apache2
run localhost in your browser
move your directory to apache's documents directory
sudo mv server /var/www/html
edit apache configuration files
sudo nano /etc/apache2/sites-available/000-default.conf
edit it thus:
ServerAdmin webmaster@localhost
        #DocumentRoot /var/www/html

        WSGIScriptAlias / /var/www/html/server/app.wsgi
        <Directory /var/www/html/server>
        Order allow,deny
        Allow from all
        </Directory>
we will create app.wsgi file
install a file for wsgi
sudo apt install libapache2-mod-wsgi-py3

restart apache
sudo service apache2 restart

cd into your directory
cd /var/www/html/server

create wsgi file
sudo nano app.wsgi

edit your __init__.py file to
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
@app.route("/home")
def home():
    return render_template("index.html")

#if __name__ == "__main__":
#    app.run(debug=True)



restart apache2
sudo service apache2 restart

reload your localhost on browser



sudo python __init__.py 
sudo nano /etc/apache2/sites-available/FlaskApp
sudo a2ensite FlaskApp

```


### Installing Ngrok
[Ngrok](https://ngrok.com/docs) is a service which allows us expose our localhost's port to the internet (public). We download from ngrok.com and extract the files in the folder to a location of choice on our machine and then run the following commands:

sudo snap install ngrok
We add our auth token to our ngrok configuration file by running
```
./ngrok authtoken random-texts
```
Example:

![image](https://user-images.githubusercontent.com/49791498/108274592-64221680-7175-11eb-827b-9905de025c78.png)

*On the download page of ngrok, an authtoken will be given to us as part of the instructions to follow. the authtoken will be added to our default ngrok.yml configuration file.*

Next we start up ngrok, to start a HTTP tunnel forwarding to our local port 80, by running:

```
ngrok http 8080
```
Expected result:
![image](https://user-images.githubusercontent.com/49791498/108285742-0c40db00-7188-11eb-9b30-e6d0a7aa793e.png)
*Copy and paste any of the links provided in your browser*

Ngrok provides a UI where we can monitor traffic running over our tunnels. In our browser, we type: ```http://localhost:4040```. To see the page in action, send a request to your webpage.
Expected result (something similar):


To access the website at a later time, 
```
cd into your directory
run
vagrant up
vagrant reload
ngrok http 80
```

### Reading Resorces:

VagrantFile: https://www.virtualbox.org/manual/ch01.html