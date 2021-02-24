1 ssh into ubuntu VM
2 i am using ubuntu 18.04 with python 3, so i run the following first
sudo add-apt-repository universe
sudo apt-get update
3 install dependencies
sudo apt install python3-pip python3-dev build-essential libssl-dev libffi-dev python3-setuptools 

4 sudo apt install python3-venv

5 create python virtual environment
python3.6 -m venv (venvname) venv

create the directory /var/www/html by
sudo mkdir /var/www/html

6 activate it source (venvname)/bin/activate

change directory to /var/www/html

install wheel by pip install wheel

install gunicorn
pip install gunicorn flask

create a python file give it a unique name
sudo nano main.py

install flask
sudo -H pip3 install flask


![image](https://user-images.githubusercontent.com/49791498/108813345-224c0280-75b1-11eb-90e5-42080567b583.png)

to disable venv run
deactivate

logout of vm by
exit

to run at a later time,
cnnect to your vm
activate venv

create wsgi.py file
```
from main import app
if __name__ == "__main__":
    app.run()
```

sudo apt-get update
sudo apt-get install nginx
nginx -v

![image](https://user-images.githubusercontent.com/49791498/108804635-1e62b500-759e-11eb-8a99-904c606f967b.png)

stop nginx
sudo systemctl stop nginx

reload nginx
sudo systemctl reload nginx

grant nginx access through the firewall
check available applications:
sudo ufw app list

allow nginx
sudo ufw allow 'nginx full'

in setting up UFW, check out this [article](https://linoxide.com/firewall/guide-ufw-firewall-ubuntu-16-10/) for when you get stuck

sudo ufw reload

![image](https://user-images.githubusercontent.com/49791498/108806114-16584480-75a1-11eb-855e-e3925f952f9f.png)

start nginx
sudo systemctl start nginx

in your browser type ```http://127/0.0.1```, nginx homepage will be displayed

install wheel
pip install wheel

install uwsgi
pip install uwsgi flask

sudo ufw allow 5000

ensure uwsgi can serve our app
    uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app

create project.ini
[uwsgi]
module = wsgi:app


master = true
processes = 5

socket = myproject.sock
chmod-socket = 660
vacuum = true


die-on-term = true

create a file ending with .service
sudo nano /etc/systemd/system/myproject.service

start uwsgi service
sudo systemctl start myproject
sudo systemctl enable myproject

check uwsgi status
sudo systemctl status myproject

configure nginx
sudo nano /etc/nginx/sites-available/myproject

create a file/etc/nginx/sites-available/myproject

server {
    listen 80;
    server_name your_domain www.your_domain;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/home/sammy/myproject/myproject.sock;
    }
}

link file to sites-enabled
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled

if you run into errors,check [here](https://www.digitalocean.com/community/questions/nginx-server-unable-to-start-up-due-to-issues-with-conf)

check for errors
sudo nginx -t

restart nginx
sudo systemctl restart nginx

adjust firewall
sudo ufw delete allow 5000
sudo ufw allow 'Nginx Full'

navigate to your server's domain name or IP address

add ssl
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python-certbot-nginx
sudo certbot --nginx -d your_domain -d www.your_domain
sudo ufw delete allow 'Nginx HTTP'

bind gunicorn to our app
sudo apt install gunicorn
gunicorn --bind 0.0.0.0:5000 wsgi:app





