#!/bin/bash
echo "----------------------------------------------------------------"
echo "Setting Up Your Hub W/ Zigbee2MQTT - Ver 1.0"
echo "----------------------------------------------------------------"
echo " "
# Node setup
sudo curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs git make g++ gcc
echo ">>>>> Node Installed <<<<<"
# Z2M setup
echo ">>>>> Clone Zigbee2MQTT repository <<<<<"
sudo mkdir /opt/zigbee2mqtt
sudo git clone https://github.com/Koenkk/zigbee2mqtt.git /opt/zigbee2mqtt
echo " "
echo ">>>>> Install dependencies <<<<<"
cd /opt/zigbee2mqtt
git fetch
git checkout master
git pull
npm ci
echo " "
echo ">>>>> Zigbee2MQTT Permission <<<<<"
sudo chown -R ${USER}: /opt/zigbee2mqtt
echo " "
echo ">>>>> Create configuration.yaml <<<<<"
cat > /opt/zigbee2mqtt/data/configuration.yaml <<EOL
# Home Assistant integration (MQTT discovery)
homeassistant: false

# Allow new devices to join
permit_join: true

# MQTT settings
mqtt:
  # MQTT base topic for Zigbee2MQTT MQTT messages
  base_topic: zigbee2mqtt
  # MQTT server URL
  server: 'mqtt://10.0.0.17:1883'
  # MQTT server authentication
  user: 'ashraf'
  password: 'AshRazan1411Atshy79'

# Serial settings
serial:
  # Location of the adapter
  port: /dev/ttyUSB0

frontend:
  # Port for the web interface
  port: 8080
  # IP address of the device running Zigbee2MQTT
  host: 0.0.0.0

advanced:
  log_level: debug
  network_key: GENERATE
EOL
echo " "
echo ">>>>> configuration.yaml file created <<<<<"
echo " "
echo ">>>>> Enable daemon with systemctl <<<<<"
cat > /etc/systemd/system/zigbee2mqtt.service <<EOL
[Unit]
Description=zigbee2mqtt
After=network.target

[Service]
ExecStart=npm start
WorkingDirectory=/opt/zigbee2mqtt
StandardOutput=inherit
StandardError=inherit
Restart=always
User=$(whoami)

[Install]
WantedBy=multi-user.target
EOL
echo " "
echo "Zigbee2MQTT Installed"
echo " "
echo ">>>>> Start Zigbee2MQTT automatically on boot <<<<<"
echo " "
sudo systemctl enable zigbee2mqtt.service
sudo reboot
