# myhasst
my hass notes

this is the inspiration: https://github.com/sjorsjes/home-assistant#install-mysql-db

# short version

Run AIO install from the pi, not ssh.  ssh sessions drop causing lots of install issues: 
  https://home-assistant.io/getting-started/installation-raspberry-pi-all-in-one/
  
After AOI install:
- update wifi and locale
- ssl
- mysql
- pycrypto (for broadlink) https://community.home-assistant.io/t/broadlink-rm-pro-getting-error/7877/14
- phantomjs (for alarm.com) https://www.reddit.com/r/homeassistant/comments/57amjb/alarmcom_homeassistant/

# detailed steps

## update wifi and locale

- sudo nano /etc/default/keyboard
- sudo dpkg-reconfigure locales
- sudo dpkg-reconfigure tzdata
- sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
add to the end of the file:
```
network={
        ssid="SSIDGOESHERE"
        psk="PASSWORDGOESHERE"
        }
```

## LetEncrypt
* [Installation guide](https://home-assistant.io/blog/2015/12/13/setup-encryption-using-lets-encrypt/)  
* After installation:  
  ```$ sudo chmod -R 777 /etc/letsencrypt/archive```  
  ```$ sudo chmod -R 777 /etc/letsencrypt/live```
  
## Install MySQL DB
```$ sudo apt-get update && sudo apt-get upgrade```  
```$ sudo apt-get install mysql-server && sudo apt-get install mysql-client```  
```$ sudo apt-get install libmysqlclient-dev```  
```$ sudo apt-get install python-dev python3-dev```  
```$ mysql -uroot -p```  
```$ CREATE DATABASE dbname;```  
```$ CREATE USER 'dbuser'@'localhost' IDENTIFIED BY 'password';```  
```$ GRANT ALL PRIVILEGES ON dbname.* TO 'dbuser'@'localhost';```  
```$ FLUSH PRIVILEGES;```  

**Test if user works:**  
```$ mysql -udbuser dbname -p```

**Switch to HASS env**  
```$ ssh pi@your_raspberry_pi_ip```  
```$ sudo su -s /bin/bash homeassistant```  
```$ source /srv/homeassistant/homeassistant_venv/bin/activate```  
```$ pip3 install --upgrade mysqlclient```  

**Add to configuration.yaml**  
```yaml
recorder:
  db_url: mysql://dbuser:password@localhost/dbname
```
  
## Install pycrypto
**Switch to HASS env**  
```$ ssh pi@your_raspberry_pi_ip```  
```$ sudo su -s /bin/bash homeassistant```  
```$ source /srv/homeassistant/homeassistant_venv/bin/activate```  
```$ pip3 install pycrypto```

## Install phantomjs
* [Installation guide](https://www.bitpi.co/2015/02/10/installing-phantomjs-on-the-raspberry-pi/) 
```
cd ~
git clone https://github.com/piksel/phantomjs-raspberrypi.git
chmod -x ~/phantomjs-raspberrypi/bin/phantomjs
chmod 775 ~/phantomjs-raspberrypi/bin/phantomjs
sudo ln -s /home/pi/phantomjs-raspberrypi/bin/phantomjs /usr/bin/
sudo apt-get install libfontconfig
```
**Do a version check**
```
phantomjs -v
1.9.8
```

