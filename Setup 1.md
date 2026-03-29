update
```
sudo apt update -y && sudo apt upgrade -y
```

`Take Snapshot 1`

---
Change power manager settings
system suspend never

display never

screen saver 0 off

add root terminal to desktop - this saves time 

---
Change terminal settings

`Appearance Tab`
increase terminal font to 20
color scheme changed to linux
checkbox draw a border

---

install addons 

foxyproxy
https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/
make a profile for burp
add profile 
title:burp
hostname:127.0.0.1
port:8080

install burp cert
http://burp/cert
firefox
settings> privacy & security > view certificates > authorities > import

wappalyzer
https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/

builtwith
https://addons.mozilla.org/en-US/firefox/addon/builtwith/

hack-tools
https://addons.mozilla.org/en-US/firefox/addon/hacktools/

dark reader
https://addons.mozilla.org/en-US/firefox/addon/darkreader/

---
setup burp suite

start burp suite
settings > user interface > display > dark
font size > 14

---

setup bookmarks

---
install tools
mkdir -p /opt/tools/privesc
mkdir -p /opt/tools/webshells
mkdir -p /opt/tools/pivoting

wget 
linpeas
winpeas


apt install seclists -y

gzip -d /usr/share/wordlists/rockyou.txt

consider installing gedit

---
usefull paths
/usr/share/windows-binaries


usefull commands 
in terminal

CTRL + SHFIT + R 
splits the terminal down the midlle

CTRL + SHFIT + D
splits  across the middle left to right

CTRL + d 
deletes last split

CTRL + t
new terminal tab

---
Virtual machine management settings,

consider creating a shared folder for the vms to access because sometime there are issue with copy pasting etc between the host and kali machine, 

ensure copy and paste is set to bidirectional

---
take a snapshot 2