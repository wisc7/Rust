# Rust
# Commands to create a rust server on Centos (9)

# <based on https://www.rustafied.com/how-to-host-a-rust-server-in-linux
# and steamcmd - https://developer.valvesoftware.com/wiki/SteamCMD#Linux>

# run the following commands: (details of these commands can be found in the steamcmd docco)



sudo useradd -m steam  
sudo passwd steam  

# unnecessary?
sudo -u steam -s  
cd /home/steam  
  
exit  
# end unnecessary?
  
sudo yum install -y glibc.i686 libstdc++.i686 nano tar  
  
sudo -iu steam  
mkdir ~/Steam && cd ~/Steam  
  
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -  
  
./steamcmd.sh +login anonymous +force_install_dir /home/steam/rust_dedicated +app_update 258550 +quit  
  
# Installed - create a new starup file with options
    
#create a file:  
nano ~/rust_dedicated/startrust.sh  
  
#past the following into file modifying with your values.

#!/bin/sh  
clear while : do  
  exec ./RustDedicated -batchmode -nographics \  
  -server.ip IPAddressHere \  
  -server.port 28015 \  
  -rcon.ip IPAddressHere \  
  -rcon.port 28016 \  
  -rcon.password "rcon password here" \  
  -server.maxplayers 75 \  
  -server.hostname "Server Name" \  
  -server.identity "my_server_identity" \  
  -server.level "Procedural Map" \  
  -server.seed 12345 \  
  -server.worldsize 3000 \  
  -server.saveinterval 300 \-server.globalchat true \  
  -server.description "Description Here" \  
  -server.headerimage "512x256px JPG/PNG headerimage link here" \  
  -server.url "Website Here"  
  echo "\nRestarting server...\n" done  

--save file
chmod 755 ~/rust_dedicated/startrust.sh 

# Firewall
sudo firewall-cmd --add-port=28016/tcp --permanent
sudo firewall-cmd --add-port=28015/tcp --permanent

