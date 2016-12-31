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

# update wifi and locale

- sudo nano /etc/default/keyboard
- sudo dpkg-reconfigure locales
- sudo dpkg-reconfigure tzdata
- sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
add to the end of the file:
network={
        ssid="SSIDGOESHERE"
        psk="PASSWORDGOESHERE"
        }

