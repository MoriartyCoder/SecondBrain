## Sudo user
When you work on Linux systems it's very dangerous to be root because one fatfingered command can destroy everything. Because of this it is always the best to work with at lowes privilages as possible. So it is an good idea to have an second administative account with root-privilages.
To do that you have to add the user to the sudo group (while having root-privilages).
```
usermod -aG sudo <USER>
```
Now the user can execute commands with root-privilages. Start the command with sudo and then write the command behind it.

## Autoupdate
In case you have a server with low importance and low care-time you can install an package for auto-updates. But be careful because updates can break things and then the server does not work anymore! To set it up do the following:
```
apt install unattended-upgrades   # If it is not already installed
dpkg-reconfigure --priority=low unattended-upgrades  # Answer the pop-up with yes
```

## Remote administration
With [[SSH#(Secure) Configuration of the server]] you can  