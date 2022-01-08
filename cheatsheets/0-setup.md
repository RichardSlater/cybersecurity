Machine Setup
=============

I routinely install and use these commands when practicing CTFs:

rustscan
```
wget https://github.com/RustScan/RustScan/releases/download/2.0.1/rustscan_2.0.1_amd64.deb
sudo dpkg -i rustscan_2.0.1_amd64.deb
```

SecLists (installed to `/usr/share/seclists/`)
```
sudo apt install seclists
``` 

Install wordlists (installed by default on Kali, but not Parrot, installed to `/usr/share/wordlists`)
```
sudo apt-get install wordlists
```

Build a container to do privesc
```
sudo apt update
sudo apt install -y golang-go debootstrap rsync gpg squashfs-tools
git clone https://github.com/lxc/distrobuilder $HOME/distrobuilder
cd $HOME/distrobuilder
make
mkdir -p $HOME/ContainerImages/alpine/
cd $HOME/ContainerImages/alpine/
wget https://raw.githubusercontent.com/lxc/lxc-ci/master/images/alpine.yaml
sudo $HOME/go/bin/distrobuilder build-lxd alpine.yaml -o image.release=3.8
```

RsaCtfTool
```
git clone https://github.com/Ganapati/RsaCtfTool.git ~/RsaCtfTool
sudo apt-get install libgmp3-dev libmpc-dev -y
cd RsaCtfTool
pip3 install -r "requirements.txt"
```

Instal jwt_tool
```
git clone https://github.com/ticarpi/jwt_tool ~/jwt_tool
cd ~/jwt_tool
python3 -m pip install termcolor cprint pycryptodomex requests
chmod 744 ~/jwt_tool/jwt_tool.py
PATH=$PATH;~/jwt_tool/jwt_tool.py
```

Update metasploit
```
sudo apt update
sudo apt install metasploit-framework -y
```

Install ghidra
```
sudo apt update
sudo apt install ghidra -y
```

Install ret2libc
```
git clone https://github.com/i4bdullah/auto-ret2libc.git
cd auto-ret2libc
pip3 install -r requirements.txt
```

Install pwntools
```
apt update
apt install -y python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential gdbserver gdb-peda
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pwntools keystone-engine ropper
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "DONE! debug your program with gdb and enjoy"
```

Environment Setup
=================

Create a projects folder
```
mkdir ~/projects
```

Create a project
```
mkdir ~/projects/PROJECT_NAME
```

I store some key variables in a `.env` file stored in a project folder:

```
RHOSTS=<IPs to scan>
RHOST=<Target IP>
RPORT=<Target Port>
RPROTOCOL=http
RURL=$RPROTOCOL://$RHOST:$RPORT/
LHOST=`ip a | grep eth0 | awk -v RS='([0-9]+\\.){3}[0-9]+/[0-9]+' 'RT{print RT}' | sed -r 's/[^0-9.]+/\n/' | head -1`
LPORT=4343
WPSCAN_API_TOKEN=[TOKEN]
```

This `.env` can be imported through the following command:
```
source .env
```