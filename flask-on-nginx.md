<<<<<<< HEAD
Hi there, today we will be looking at how to host a Flask app on Nginx and an Ubuntu Virtual Machine.

[Nginx](https://nginx.org/en/docs/beginners_guide.html) is a webserver that can be used as reverse proxy, load balancer, mail proxy and HTTP Cache. It is widely used due to it's ability to scale webites better.
A webserver is a computer that has web server software installed (Apache, Nginx, Microsoft IIS) and serves webpages over the internet.


1 ssh into ubuntu VM
2 i am using ubuntu 18.04 with python 3, so i run the following first
sudo add-apt-repository universe
sudo apt-get update
3 install dependencies
sudo apt install python3-pip python3-dev build-essential libssl-dev libffi-dev python3-setuptools 
=======
# landing-page
Login Page hosted on a linux server
>>>>>>> d71285572fe6e65d57fcb48e2f8b70b8b343f4be

Steps to take in running this project:

*   Clone the project
*   Create a virtual environment by running:
    ```
    python -m venv <name of virtual environment>
    ```
Example:
```
python -m venv venv
```
*   Activate your virtual environment:
    ```
    source <venvname/bin/activate>
    ```
Example:
```
source venv/bin/activate
```

*   Install the requirements
    ```
    pip install -r requirements.txt
    ```

*   Run the project by typing the following in your terminal:
    ```
    export FLASK_APP=run.py
    export FLASK_ENV=development
    flask run
    ```

*   Type in your browser:
    ```
    127.0.0.1:5000/
    ```
*   Next, we check if our project runs with uwsgi
    ```
    uwsgi dev.ini
    ```
    In your browser type:
    ```
    127.0.0.1:9090/
    ```

## In your VM

ssh into a vm
sudo apt update
install [python](https://phoenixnap.com/kb/how-to-install-python-3-ubuntu)
clone the repo into your VM
```
sudo apt update
git clone <repo-url>
```

change your directory's name to app
```
mv <repo-name>/ app
```
Then cd into your file and create a virtual environment and activate it
```
sudo apt-get install python3-venv
python3 -m venv <virtual-environment-name>
source <virtual-environment-name>/bin/activate
```
    
Install the requirements in your requirements.txt file
```
    pip3 install -r requirements.txt
```
*run ```pip3 list``` to see a list of all installed dependencies*
If uWSGI does not install, run the following;
```
sudo apt-get install build-essential python3-dev
pip3 install uwsgi
```
    
Now, to check if your app runs on uwsgi, but first we have to set our ufw to allow for connections on port 9090
  
```
sudo ufw enable
sudo ufw allow 9090
```
Expected result:

![image](https://user-images.githubusercontent.com/49791498/109410162-9cf19500-7998-11eb-8a45-18690d12b2cd.png)

   
*Running ```sudo ufw status``` shows us the crrent state of our firewall. if it is active or not and the ports our firewall gives us access to.*
    
go ahead and run 
```
uwsgi dev.ini
```
* You might see the following in the output:
![image](https://user-images.githubusercontent.com/49791498/109412657-8acc2280-79a9-11eb-8df8-7192e52c77b1.png)

run, 
```
sudo apt-get install libpcre3 libpcre3-dev
pip3 install uwsgi -I --no-cache-dir
```
re-run the command 
```
uwsgi dev.ini
```
and in your browser type 
```
<vmipaddress>:9090/
```

delete firewall
```
sudo ufw delete allow 9090
```

deactivate virtual environment
```
deactivate
```

install nginx
```
sudo apt install nginx
```
check the apps recognized by ufw
sudo ufw app list
allow connections to http
sudo ufw allow 'Nginx HTTP'
in your browser, type your VM's IP address
Expected result:
![image](https://user-images.githubusercontent.com/49791498/109413045-c1a33800-79ab-11eb-900f-8de337b31f7d.png)

create systemd unit file: to reboot the server running our app
```
sudo nano /etc/systemd/system/<custom-name-of-service-app>.service
```
sudo nano /etc/systemd/system/app.service
```
[Unit]
Description=A simple Flask uWSGI application
After=network.target

[Service]
User=<yourusername>
Group=www-data
WorkingDirectory=/home/<yourusername>/app
Environment="PATH=/home/<yourusername>/app/env/bin"
ExecStart=/home/<yourusername>/app/env/bin/uwsgi --ini <name-of-app-ini file>

[Install]
WantedBy=multi-user.target
```

to enable the service file just created, run
sudo systemctl start <name-of-app>
sudo systemctl enable app <name-of-app>
expected output:
![image](https://user-images.githubusercontent.com/49791498/109413425-93bef300-79ad-11eb-81cd-36a0b6c4d644.png)

check if the file is running:
sudo systemctl status app

if you make a mistake in your service file and correct it, you have to reload daemon and restart systemctl
![image](https://user-images.githubusercontent.com/49791498/109413575-6fafe180-79ae-11eb-9ef5-9a29d239f77c.png)

configure nginx
sudo nano /etc/nginx/sites-available/app
```
server {
    listen 80;
    server_name <your_ip_address> <your_domain_name>;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/home/<username>/app/app.sock;
    }
}
```
check if there are any errors in your nginx configuration
sudo nginx -t
![image](https://user-images.githubusercontent.com/49791498/109413763-6ecb7f80-79af-11eb-8d01-3c64664d7dfe.png)

link app file to sites available
sudo ln -s /<path-to-config-file> /etc/nginx/sites-enabled
sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/

you can run a sytax check again, to be sure there are no errors

restart nginx for changes to take place
sudo systemctl restart nginx

in your browser, type in your VM IP address without adding a port becase in nginx config file, we have defined a port for nginx to listen on

tada!!
![image](https://user-images.githubusercontent.com/49791498/109413975-9ec75280-79b0-11eb-8859-0fa7096da98a.png)
*IP address*

![image](https://user-images.githubusercontent.com/49791498/109420276-d1357780-79d1-11eb-870a-4ee7a331b278.png)
*Azure custom domain name*

*cool thing is, anyone with your public IP address can access your webpage*



[Github repo](https://github.com/Mbaoma/landing-page)
[Linkedin](https://www.linkedin.com/in/mbaoma-chioma-mary)