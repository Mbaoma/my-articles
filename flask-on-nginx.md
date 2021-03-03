Hi there, today we will be looking at how to host a Flask app on Nginx and an Ubuntu Virtual Machine.

[Nginx](https://nginx.org/en/docs/beginners_guide.html) is a webserver that can be used as reverse proxy, load balancer, mail proxy and HTTP Cache. It is widely used due to it's ability to scale webites better.
[A webserver](https://en.wikipedia.org/wiki/Web_server) is a computer that has web server software installed (Apache, Nginx, Microsoft IIS) and serves webpages to fulfill client's requests over the internet.

The first thing, we do is to [provision](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal) an Ubuntu Virtual Machine (I use Microsoft Azure) and then login to it via SSH.

*(We assume we have a Flask application already built and on Github)*

We then run the following command to update our repositories:
```
sudo apt update
```

Dependig on the version of your Ubuntu VM, you might want to [upgrade](https://docs.python-guide.org/starting/install3/linux/) your Python version.
After this, we continue with the steps below:

*   We clone our app's GitHub repository into our VM:
```
sudo apt update
git clone <repo-url>
```

*   We change our directory's name to ```app```
```
ls
mv <repo-name>/ app
```

*   We cd into our directory, create a virtual environment and activate it
```
sudo apt-get install python3-venv
python3 -m venv <virtual-environment-name>
source <virtual-environment-name>/bin/activate
```
    
*   We install the requirements in our ```requirements.txt``` file
```
    pip3 install -r requirements.txt
```

*run ```pip3 list``` to see a list of all installed dependencies*

If uWSGI does not install, run the following;
```
sudo apt-get install build-essential python3-dev
pip3 install uwsgi
```

So far, so good!

Now, we check if our app runs on uwsgi, but first we have to set our Uncomplicated Fire Wall (ufw) to allow both incoming and outgoing connections on port 9090. 
  
```
sudo ufw enable
sudo ufw allow 9090
```
Expected result:

![image](https://user-images.githubusercontent.com/49791498/109410162-9cf19500-7998-11eb-8a45-18690d12b2cd.png)

   
*Running ```sudo ufw status``` shows us the current state of our firewall; whether it is active or not and the ports our firewall gives us access to.*
    
*   Now, we run:
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

then re-run the command 
```
uwsgi dev.ini
```

and in your browser type 
```
<vmipaddress>:9090/
```
We expect to see our Flask app displayed via this port.

*   Then we delete the firewall rule and deactivate our virtual environment
```
sudo ufw delete allow 9090
deactivate
```

*   We go ahead to install nginx by running:
```
sudo apt install Nginx
```
Now, we check the apps recognized by ufw and instruct it to allow for connection to Nginx:
```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
```

To confirm if Nginx was properly installed, we type our VM's IP address in our browser.
Expected result:
![image](https://user-images.githubusercontent.com/49791498/109413045-c1a33800-79ab-11eb-900f-8de337b31f7d.png)

Omoshiroi......

*   Up next, we create systemd unit file to reboot the server running our app
```
sudo nano /etc/systemd/system/<custom-name-of-service-app>.service
```
Example:
```
sudo nano /etc/systemd/system/app.service
```
And type in the following contents in our file:
```
[Unit]
Description=A simple Flask uWSGI application
After=network.target

[Service]
User=<yourusername>
Group=www-data
WorkingDirectory=/home/<yourusername>/app
Environment="PATH=/home/<yourusername>/app/venv/bin"
ExecStart=/home/<yourusername>/app/venv/bin/uwsgi --ini <name-of-app-ini file>

[Install]
WantedBy=multi-user.target
```

*   We enable the service file we just created, by running:
```
sudo systemctl start <name-of-app>
sudo systemctl enable app <name-of-app>
```

Expected output:
![image](https://user-images.githubusercontent.com/49791498/109413425-93bef300-79ad-11eb-81cd-36a0b6c4d644.png)

*To check if the file is running, type:*
```
sudo systemctl status app
```
*Also if a mistake is made in our service file and we correct it, we have to reload the daemon and restart systemctl*
![image](https://user-images.githubusercontent.com/49791498/109413575-6fafe180-79ae-11eb-9ef5-9a29d239f77c.png)

*   Now, we configure nginx by creating a config file
```
sudo nano /etc/nginx/sites-available/<name-of-file>
```
And type in the following contents:
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

*To check if there are any errors in our nginx configuration file, we run*
sudo nginx -t
![image](https://user-images.githubusercontent.com/49791498/109413763-6ecb7f80-79af-11eb-8d01-3c64664d7dfe.png)

*   We link our config file to sites available:
```
sudo ln -s /<path-to-config-file>/<config-file-name> /etc/nginx/sites-enabled
```
Example:
```
sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/
```

*Running a syntax check at this point, comes in handy*

*   We restart nginx for our changes to take effect
```
sudo systemctl restart nginx
```
*In our browser, we type in our VM's IP address without adding a port becase in our nginx config file, we have defined a port for nginx to listen on*

tada!!
![image](https://user-images.githubusercontent.com/49791498/109413975-9ec75280-79b0-11eb-8859-0fa7096da98a.png)
*IP address*

![image](https://user-images.githubusercontent.com/49791498/109420276-d1357780-79d1-11eb-870a-4ee7a331b278.png)
*Azure custom domain name*

*Feel free to share your IP address with friends to check out your app*

Thank you for your time!!!

Find below the GitHub repo for this task:
[Github repo](https://github.com/Mbaoma/landing-page)

Feel free to connect with me via [Linkedin](https://www.linkedin.com/in/mbaoma-chioma-mary)

Gokigen'yō


