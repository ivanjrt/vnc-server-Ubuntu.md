# Content: 
# This will talk about the steps to create VNC Server within Ubuntu 23.10 (This might work on earlier versions but I didn't get to test that)
This Involves security deal, where you are leaving the computer unlocked, You have been warned and its up to you to follow the steps below.


* Installing The Display Manager &  Restarting
```bash
sudo apt update
sudo apt install lightdm
sudo reboot
```

* Instaling the VNC Serverver and Service.
```bash
sudo apt install x11vnc -y
```
To create it as a service so whenever the machine restarts then the VNC becomes active. <br/>
start by create this file `sudo nano /lib/systemd/system/x11vnc.service`  <br/>
this will open up Nano, and paste the below.  <br/>
Note: Replace the `passw0rd` with your own.  <br/>
```bash
[Unit]
Description=x11vnc server service
After=display-manager.service network.target syslog.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -forever -display :0 -auth guess -passwd passw0rd
ExecStop=/usr/bin/killall x11vnc
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
save and close the file <br/> <br/>

* We need to have the systemd to adknowledge the service.
```bash
systemctl daemon-reload
```
start the service: <br/>
```bash
systemctl start x11vnc.service
```
then lets see if the service is running:  <br/>
```bash
systemctl status x11vnc.service
```
If this gives you the greencheck that is running, then now we enable,   <br/>
```bash
systemctl enable x11vnc.service
```

# Things to consider:
* Leave the computer unlocked, then remote In otherwise even though you might bypass the VNC's server password and you are able to type the Ubuntu's password machine, <br/>
it will not let you IN (_at least from my expierence_), therefore:  <br/>
![image](https://github.com/ivanjrt/vnc-server-Ubuntu.md/assets/44326428/5c64145a-b2da-4abe-b32e-1bb5be1f70f7)  <br/>
