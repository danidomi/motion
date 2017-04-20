Motion
=============

## Motin Configuration in Raspberry Pi
Download only for WINDOWS - https://sourceforge.net/projects/win32diskimager

Download raspbian link - https://downloads.raspberrypi.org/raspbian_lite_latest

After writting the image to SD Card, from the command line login using pi as username and raspberry as password



```
>wget https://github.com/danidomi/motion/releases/download/release/motion.zip
>unzip motion.zip
>cd mmal
>sudo apt-get install -y libjpeg-dev libavformat56 libavformat-dev libavcodec56 libavcodec-dev libavutil54 libavutil-dev libc6-dev zlib1g-dev libmysqlclient18 libmysqlclient-dev libpq5 libpq-dev
>sudo apt-get install nginx
>sudo apt-get install apache2-utils
>sudo htpasswd -c /home/pi/.htpasswd admin 
```

Add a cronjob to check if the motion camera is online
 
```
>sudo crontab -e 
*/1 * * * * pgrep motion || /home/pi/mmal/startmotion
```

You wont even need to start it


## Nginx Configuration in Raspberry Pi

>sudo apt-get install nginx
>sudo apt-get install apache2-utils
>sudo htpasswd -c /home/pi/.htpasswd admin
>vi /etc/nginx/sites-available/default


Add the following code below to it
```
location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                auth_basic “Restricted”;
                auth_basic_user_file /home/pi/.htpasswd;
                proxy_pass http://127.0.0.1:8081/img/video.mjpeg;
        }
```

And start the service

```
>sudo service nginx start
```

## FreeDNS Configuration in Raspberry Pi

Create an account if you don't have it 

```
>vi ~/updatedns.sh
```

Copy the below content to it.

```
#!/bin/sh
wget --no-check-certificate -O - https://freedns.afraid.org/dynamic/update.php?XXXXXXXXXXXXX >> /tmp/XXXXXXXXXXXXX.log 2>&1 &
```

## Android Configuration in Raspberry Pi

To follow up your Raspberry Pi Camera from any place, you just need to have internet and the following application:

https://play.google.com/store/apps/details?id=de.twolazy.monitor

Just configure with the information above and enjoy
