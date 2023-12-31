# Rust
# Commands to create a rust server on Centos (9)

#based on https://www.rustafied.com/how-to-host-a-rust-server-in-linux  
#and steamcmd - https://developer.valvesoftware.com/wiki/SteamCMD#Linux  
#and https://wiki.facepunch.com/rust/Creating-a-server  
  
# run the following commands: (details of these commands can be found in the steamcmd docco)

sudo yum install -y glibc.i686 libstdc++.i686 nano tar  

sudo useradd -m steam  
sudo passwd steam   
sudo -iu steam  
mkdir ~/Steam && cd ~/Steam  
  
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -  
  
./steamcmd.sh +login anonymous +force_install_dir /home/steam/rust_dedicated +app_update 258550 +quit  
  
#Rust is now installed
  
# Configure - create a new startup file with options for rust  
#create a file:  
nano ~/rust_dedicated/startrust.sh  
  
#past the following into file modifying with your values.

#!/bin/sh  
clear while : do  
  exec ./RustDedicated -batchmode \  
  -server.ip 0.0.0.0 \  
  -server.queryport 28014 \
  -server.port 28015 \  
  -rcon.port 28016 \  
  -app.port 28017 \
  -rcon.password "rcon password here" \  
  -server.maxplayers 75 \  
  -server.hostname "Server Name" \  
  -server.identity "my_server_identity" \  
  -server.level "Procedural Map" \  
  -server.seed 12345 \  
  -server.worldsize 3000 \  
  -server.saveinterval 300 \  
  -server.globalchat true \  
  -server.description "Description Here" \  
  -server.headerimage "512x256px JPG/PNG headerimage link here" \  
  -server.url "Website Here"  \
  -logfile "rustlog.txt"  \
  -hackablelockedcrete.requiredhackseconds \
  -rcon.web 1
  echo "\nRestarting server...\n" done  

--save file and exit

chmod 755 ~/rust_dedicated/startrust.sh 

# Firewall
sudo firewall-cmd --add-port={28014/udp,28015/udp,28016/tcp,28017/tcp} --permanent  
sudo firewall-cmd --reload  

#modify any network firewall/router/nat with the above ports as requried.

# Start rust
screen -S rust_server ./startrust.sh
