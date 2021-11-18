# How to setup Jetson nano JetPack4.5.1 L4T 3.5.1


## Setup Remote Connection - VNC Server

```sh
cd /usr/lib/systemd/user/graphical-session.target.wants
sudo ln -s ../vino-server.service ./.

gsettings set org.gnome.Vino prompt-enabled false
gsettings set org.gnome.Vino require-encryption false

# Replace thepassword with your desired password
gsettings set org.gnome.Vino authentication-methods "['vnc']"
gsettings set org.gnome.Vino vnc-password $(echo -n 'thepassword'|base64)

sudo reboot
```


## Prepare working directories

```sh
mkdir -p ~/repos
mkdir -p ~/workspace
```


## Setup Jetson Utilities

```sh
cd ~/repos
git clone https://github.com/jetsonhacks/jetsonUtilities

python jetsonInfo.py
```


## Setup Jetson GPU Graph Monitor

```sh
cd ~/repos
git clone https://github.com/jetsonhacks/gpuGraphTX.git
cd gpuGraphTX/

sudo apt install -y python-matplotlib

python ./gpuGraph.ph
```


## Connect USB-Cam




## Install DEMO - People Counting

```sh
cd ~/repos

git clone https://github.com/JardinRyu/Jetson_Nano_People_Counting
cd Jetson_Nano_People_Counting

# The installation process can take several hours. So be patient...
# ${HOME}/Jetson_Nano_People_Counting/
./install.sh

cd ssd/
# ${HOME}/Jetson_Nano_People_Counting/ssd/
./install.sh
./build_engines.sh
```


-----
EOF
