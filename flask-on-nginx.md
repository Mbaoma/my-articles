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
pip install flask


![image](https://user-images.githubusercontent.com/49791498/108813345-224c0280-75b1-11eb-90e5-42080567b583.png)

to disable venv run
deactivate

logout of vm by
exit

to run at a later time,
cnnect to your vm
activate venv







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