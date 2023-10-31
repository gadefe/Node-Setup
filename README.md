# Node-Setup


wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

sudo add-apt-repository "deb https://packages.grafana.com/enterpri... stable main"

sudo apt update

sudo apt install grafana

sudo systemctl start grafana-server

sudo systemctl status grafana-server

sudo systemctl enable grafana-server





wget -q https://repos.influxdata.com/influxdata-archive_compat.key

echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null

echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list

sudo apt-get update
sudo apt-get install influxdb

sudo systemctl unmask influxdb.service

sudo systemctl start influxdb

influxd -config /etc/influxdb/influxdb.conf

to run influx type the commd below
influx

ubuntu command to view the running instances and port

sudo lsof -i -P

https://www.youtube.com/watch?v=Vq4cDIdz_M8


https://www.youtube.com/watch?v=yHTWzFpgCuk&t=211s


# ********** Working NODERED setup ********

I started with this image:
https://downloads.raspberrypi.org/r...021-05-28/2021-05-07-raspios-buster-armhf.zip


and copied it to a SD card using https://www.balena.io/etcher/
after copying don't forget to create a ssh.txt file on the SD-card, so that putty will work :)

use putty and the IP given by your router to log in via ssh

# Install grafana:

sudo apt-get install -y apt-transport-https && sudo apt-get install -y software-properties-common wget && wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add - && echo "deb https://packages.grafana.com/enterprise/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list && sudo apt-get update && sudo apt-get install grafana-enterprise && sudo service grafana-server start && sudo systemctl start grafana-server && sudo systemctl enable grafana-server && sudo grafana-cli plugins install grafana-clock-panel

# Install influx:
sudo apt-get install influxdb && sudo service influxdb start && sudo apt install influxdb-client && sudo systemctl enable --now influxdb

start influxstart 
influx
create database batrium
create user batrium with password 'batrium'
grant all on batrium to batrium
exit

# install node-red:
sudo apt install build-essential git curl && bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered) --nodered-version="1.3.5" --node14

sudo systemctl enable nodered.service

sudo systemctl enable nodered

sudo systemctl start nodered

cd /home/pi/.node-redionflux

npm install binary-parser

start node-red once

node-red

and close it after starting => thats how the settings.js is cretaed initially


edit /home/pi/.node-red/settings.js and add after the following line: "functionGlobalContext:" => binary_parser:require('binary-parser').Parser
sudo nano /home/pi/.node-red/settings.js
so it will look like that
functionGlobalContext: {
binary_parser:require('binary-parser').Parser
// os:require('os'),
// jfive:require("johnny-five"),
// j5board:require("johnny-five").Board({repl:false})
},
reboot PI and hopefully all services will start
sudo reboot

Go to node-red page => <your.pi.ip:1880>
Add the following nodes via "manage palette"
node-red-contrib-influxdb
node-red-dashboard
import the red node flow attached here (unzipped of course). This is modified to fit to newer batrium firmware (2.14) based on UPD-listener source code by daromer.
you can debug some messges while klicking on the outer edge of the green payload nodes
=> now you influxdb should be fed by node red :->

open grafana (1st pw must be changed) => <your.pi.ip:3000>
click on tile "add you 1st data source"
choose influx
Name: Batrium
URL: http://127.0.0.1:8086
Database: batrium
user & pw = empty (no need)
save & test => all green => good to go :)

Add the UI:
Dashboard => manage => Import >= upload JSON file => use the file attached here (unzipped of course)
(one open issue is left => status of shunt is not shown, but not that vital 2me:)
===
OK, guys, thats how I managed to get it running. Hope that helps you as you helped me.
Big thanks to daromer, wolf and this awesome community !
Attachments
batrium_node_red_flow_2_14.zip
22 KB · Views: 108
Batrium-grafana_dashboard_16cell.zip
4.3 KB · Views: 105
