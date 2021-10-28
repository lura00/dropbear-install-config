## This is the start of my initial server setup instructions

Guides followed: as JuiceGhost provided:
* https://linuxconfig.org/how-to-install-and-configure-dropbear-on-linux
* https://linuxhint.com/advanced_ufw_firewall_configuration_ubuntu/
* https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands
* https://www.linuxnix.com/remove-ssh-server-linux/

# Install tasks
* Use apt to install dropbear
```
output of installing dropbear:

root@server:/home/urFolder# apt install dropbear

```
# Configuration
* Use nano to edit /etc/default/dropbear
```
root@server:/home/urFolder# nano /etc/default/dropbear
```
* In the file we've made the following changes
```
Change "NO_START=1" to "NO_START=0"
And change "DROPBEAR_PORT=between something like 20000-40000" # was 22

```
* Then restart the server and test log ing again
```
systemctl restart dropbear
```
* Next i verified from a separate local shell that dropbear is working
* and that i am able to logon using my custom port between 20000-40000
```
~ λ ssh -p xxxxx servername
...motd shows here
```
# Install howto for ufw (the firewall)
* I installed ufw on the machine as root
```
Check first if ufw is installed using ´this:
root@server:/home/urFolder# ufw status verbose
Status: inactive
else install:

root@server:/home/urfolder# apt install ufw

```
* I allow port xxxxx which is the port dropbear is configured to listen for ssh connections from
```
root@server:/home/urFolder# ufw allow xxxxx/tcp
Rules updated
Rules updated (v6)
```
* I enabled ufw and checked its status
```
root@server:/home/urFolder# ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? press Y (yes)
Firewall is active and enabled on system startup

Check ufw status verbose and see if anything changed.
root@server:/home/urFolder# ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
30042/tcp                  ALLOW IN    Anywhere
30042/tcp (v6)             ALLOW IN    Anywhere (v6)

```
* I stop the built-in SSH service
```
root@server:/home/urFolder# systemctl stop ssh
```
* I remove the openssh-server using apt
```
root@server:/home/urFolder# apt remove openssh-server
it will ask if you want to continue, press yes.
Do you want to continue? [Y/n]
(Reading database ... 55697 files and directories currently installed.)
Removing ssh (1:8.2p1-4ubuntu0.3) ...
Removing openssh-server (1:8.2p1-4ubuntu0.3) ...
Processing triggers for man-db (2.9.1-1) ...

```
* Next i reboot the server just to make sure everything still works after a fresh boot
```
root@server:/home/urFolder# shutdown -r now
```
* I try logging in again after the reboot
```
~ λ ssh -p xxxxx <server>
...
