Installation notes from when I install Kali machine for penetration testing. Focussing on On-Prem Active Directory and Infrastructur. I wrote these notes down so it is easier to reinstall my system. Posted it publicly as inspiration for others. If you have any good tool recommendations feel free to DM me on discord 0xjs#9027.

# Install Kali

#### Todo
- Keep adding stuff along the way I install stuff
- Next time reinstalling running more with git clone and copying tools out so updating tools is easier with ```sudo gitup --add /opt```

## Installed packages through apt
```
sudo apt install -y xclip sshuttle pipx docker.io docker-compose docker-clean docker-registry gedit golang wireguard stdunnel resolvconf seclists feroxbuster
sudo systemctl enable docker --now
```

## Set go path
```
echo "export GOROOT=/usr/lib/go" > ~/.zshrc
echo "export GOPATH=$HOME/go" > ~/.zshrc
echo "export PATH=$GOPATH/bin:$GOROOT/bin:$PATH" > ~/.zshrc
```

## Installed tools through pip
```
pipx install git+https://github.com/zimedev/certipy-merged.git@main
pipx install git+https://github.com/login-securite/DonPAPI.git
```

## Installed tools through git
```
sudo chown -R user:user /opt/
cd /opt

# Windows dir
mkdir windows && cd /opt/windows
wget https://github.com/ropnop/kerbrute/releases/download/v1.0.3/kerbrute_linux_amd64 -O kerbrute && chmod +x kerbrute
git clone https://github.com/zyn3rgy/LdapRelayScan && cd LdapRelayScan && virtualenv env && source env/bin/activate && pip3 install -r requirements.txt
deactivate && cd /opt/windows
mkdir sysinternals && cd sysinternals && wget https://download.sysinternals.com/files/SysinternalsSuite.zip && unzip SysinternalsSuite.zip && rm -rf SysinternalsSuite.zip && cd ../
git clone https://github.com/topotam/PetitPotam
git clone https://github.com/dirkjanm/krbrelayx
git clone https://github.com/dirkjanm/mitm6
git clone https://github.com/Greenwolf/ntlm_theft
git clone https://github.com/nccgroup/Change-Lockscreen

# Privesc section
cd /opt && mkdir privesc && cd privesc && mkdir windows && mkdir linux
cd /opt/privesc/windows && wget https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/winPEAS/winPEASbat/winPEAS.bat 
cd /opt/privesc/ && git clone https://github.com/carlospolop/PEASS-ng
wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
wget https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/raw/master/Seatbelt.exe
wget https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/raw/master/SharpUp.exe
wget https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer32.exe
wget https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer64.exe
wget https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe
https://github.com/carlospolop/PEASS-ng/releases/download/20220214/winPEASx86.exe
wget https://github.com/carlospolop/PEASS-ng/releases/download/20220214/winPEASx64.exe
git clone https://github.com/bitsadmin/wesng --depth 1 && cd wesng && python3 wes.py --update && cd ../
cp /opt/windows/sysinternals/accesschk64.exe .
cp /opt/windows/sysinternals/accesschk.exe .

cd /opt/privesc/linux
git clone https://github.com/rebootuser/LinEnum && cp LinEnum/LinEnum.sh .  && rm -rf LinEnum
git clone https://github.com/diego-treitos/linux-smart-enumeration && cp linux-smart-enumeration/lse.sh . && rm -rf linux-smart-enumeration
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh && chmod +x linpeas.sh
wget https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64
wget https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy32 && chmod +x pspy*
git clone https://github.com/jondonas/linux-exploit-suggester-2
```

## Manual tasks
- Change terminal opacity
  - Open terminal --> File --> Preferences --> Application transperancy to 0%
- Download [Burp Pro](https://portswigger.net/burp/releases#professional)
  - Run the `burp.sh` script
- Install Bloodhound
  - `wget https://raw.githubusercontent.com/SpecterOps/bloodhound/main/examples/docker-compose/docker-compose.yml && dockdocker-compose -f docker-compose.yml up && rm docker-compose.yml`
  - Locate the randomly generated password in the terminal output of Docker Compose
  - In a browser, navigate to `http://localhost:8080/ui/login`. Login with a username of `admin` and the randomly generated password from the logs
- Install tmux config
  - `cd ~/ & wget https://raw.githubusercontent.com/0xJs/tmux.conf/master/.tmux.conf`
- Foxyproxy
  - Install foxyproxy firefox addon. 

## Cleanup
```

```

#### Update github repos in /opt
```
sudo gitup --add /opt
```

#### Old
```
# Google cloud tools
cd /opt && mkdir gc && cd /opt/gc

wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-420.0.0-linux-x86_64.tar.gz
tar -xvf google-cloud-cli-420.0.0-linux-x86_64.tar.gz
bash /opt/gc/google-cloud-sdk/install.sh -q --command-completion=true --path-update=true
rm -rf google-cloud-cli-420.0.0-linux-x86_64.tar.gz

git clone https://github.com/RedTeamOperations/GCPTokenReuse

git clone https://github.com/google/gcp_scanner.git
cd gcp_scanner && pip3 install -r requirements.txt

git clone https://github.com/carlospolop/bf_my_gcp_permissions

cd /opt/gc/
git clone https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_enum

cd /opt/
git clone https://github.com/zricethezav/gitleaks
cd gitleaks && make build

cd /opt/
git clone https://github.com/initstring/cloud_enum
cd cloud_enum && pip3 install -r requirements.txt

cd /opt/gc/
git clone https://github.com/dxa4481/gcploit
alias sudo='sudo '
alias gcploit="docker run -v $(pwd)/db:/db -v $HOME/.config:/root/.config -it --rm dxa4481/gcploit python main.py"
sudo gcploit
echo 'alias gcploit="docker run -v $(pwd)/db:/db -v $HOME/.config:/root/.config -it --rm dxa4481/gcploit python main.py"' >> ~/.zshrc

cd /opt/gc
git clone https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation
cd GCP-IAM-Privilege-Escalation/PrivEscScanner
pip3 install -r requirements.txt

cd /opt/
git clone https://github.com/nccgroup/ScoutSuite
cd ScoutSuite
virtualenv -p python3 venv
source venv/bin/activate
pip3 install -r requirements.txt
```
