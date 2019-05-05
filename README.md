## Topic
  The text was created for the needs of the II project (Inżynieria internetu).

  Main goal of this tutorial is to control and monitor network services and system daemons using **systemd** in Linux systems.
  
  Daemon is a computer program that runs as a background process, rather than being under the direct control of an interactive user. Daemons in unix systems are highlighted by addyng "d" letter and the and of name e.g: DHCPD - is a DHCP server program that operates as a daemon on a server to provide Dynamic Host Configuration Protocol (DHCP).
  
  A service often refers to one or more daemons.

## Step 1
  Get linux system, any distribution (preferred Ubuntu)
## Step 2 - short about
  The status of a service can be viewed with command **systemctl**. The systemctl **systemctl** is used to manage different types of systemd - *units*. All of them can be displayed with command:
  
    aaa@ubuntu:~$ systemctl -t help
    Available unit types:
    service
    socket
    target
    device
    mount
    automount
    swap
    timer
    path
    slice
    scope


## Step 2 - preparation
  Get example of service.
  Linux has a lot of these but we try to get specific one (and useful!)
  Please, install SSH server and make basic configuration:

Open the terminal application type:

    >$ sudo apt update
    >$ sudo apt upgrade
    >$ sudo apt-get install openssh-server
Enable the ssh service by typing:

    >$ sudo systemctl enable ssh
Start the ssh service by typing:

    >$ sudo systemctl start ssh
Test it by login into the system using:

    >$ ssh user@server-name

In next steps above commands will be explained.
## Step 3 - Service states
  Service status can be viewed with command: **systemctl status name.type**  (unit type)
  
  Input: **systemctl status sshd.service**
  
    aaa@ubuntu:~$ systemctl status sshd.service
    ● ssh.service - OpenBSD Secure Shell server
       Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
       Active: active (running) since Thu 2019-05-02 08:48:50 PDT; 17min ago
      Process: 1031 ExecReload=/bin/kill -HUP $MAINPID (code=exited, status=0/SUCCESS)
      Process: 1024 ExecReload=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
      Process: 902 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
     Main PID: 922 (sshd)
        Tasks: 1 (limit: 8825)
       CGroup: /system.slice/ssh.service
               └─922 /usr/sbin/sshd -D

    May 02 08:48:49 ubuntu systemd[1]: Starting OpenBSD Secure Shell server...
    May 02 08:48:50 ubuntu sshd[922]: Server listening on 0.0.0.0 port 22.
    May 02 08:48:50 ubuntu sshd[922]: Server listening on :: port 22.
    May 02 08:48:50 ubuntu systemd[1]: Started OpenBSD Secure Shell server.
    May 02 08:48:52 ubuntu systemd[1]: Reloading OpenBSD Secure Shell server.
    May 02 08:48:52 ubuntu sshd[922]: Received SIGHUP; restarting.
    May 02 08:48:52 ubuntu sshd[922]: Server listening on 0.0.0.0 port 22.
    May 02 08:48:52 ubuntu sshd[922]: Server listening on :: port 22.
 You can meet with vairous types of service's status:
 
| State | Description |
| ------------- | ------------- |
| loaded | Unit configuration file has been processed.|
| active (running)   | Running with one or more continuing processes.|
| active (exited)   | Successfully completed a one-time configuration.|
| active (waiting)   | Running but waiting for an event.|
| inactive   | Not running.|
| enabled   | Will be started at boot time.|
| disabled   | Will not be started at boot time.|
| static | Cannot be enabled, but may be started by an enabled unit automatically.|

## Step 4 - listening and some manipulate - demonstration
  To listening all services (all type of units) in system just type **systemctl** in terminal. You will got a long list.
  
  To choose specific type of unit e.g service just type previous command with param *--type*: **systemctl --type=service**
  
    >$ systemctl --type=service
  Example:
  ![](https://i.imgur.com/YbTVJwW.png)
  
  Status argument may also be used to determine if a particular unit is active and show if the unis is enabled to start at *boot time*.
  
    aaa@ubuntu:~$ systemctl is-active sshd
    active
    aaa@ubuntu:~$ systemctl is-enabled sshd
    enabled
  List the active statf of all loaded units. There is option to limit type of unit. 
  
    >$ systemctl list-units --type=service
  
  By adding *-all* param it displays inactive units too.
 
    >$ systemctl list-units --type=service -all
  
  ![](https://i.imgur.com/5CZyIJ4.png)
  
  View the enabled and disabled settings for all units. Optionally, limit the type of unit.
  
    >$ systemctl list -unit -files --type=service
  Or just ony failed.
  
    >$ systemctl --failed --type=service
    
  ## Step 5 - Starting and stop ping system daemons on a running system
  
  View status of service: 
  
      >$ systemctl status sshd.service
  Verify process is running:
  
      >$ ps -up PID
  Stop the service and verify status:
  
      >$ systemctl stop sshd.service
  Start the service and verify status (look at process ID!!):
  
      >$ systemctl start sshd.service
  Stop and start service - restart and verify status:
  
      >$ systemctl restart sshd.service
  Or just reload and verify status (will ID process change?):
  
      >$ systemctl reload sshd.service
  
  Service which we want to stop may has dependencies:
  
    aaa@ubuntu:~$ systemctl stop cups.service
    Failed to stop cups.service: Access denied
    See system logs and 'systemctl status cups.service' for details.
    Warning: Stopping cups.service, but it can still be activated by:
      cups.socket
      cups.path
    
  To get dependencies of specific unit just type:
  
    >$ systemctl list-dependencies UNIT
    
 Masking services.
    
 ## Step 6 - Enabling system services/daemons to start or stop at boot
 Starting a service on a running systerm does not guarantee that the service will be restarted when the system reboot. Stopping service will not keep it form starting again when the system reboot. So how handle with that?
 
Verify status of the serivce

    >$ systemctl status ssh
Disable a service and check status

    >$ systemctl disable ssh
    >$ systemctl status ssh

Enable the service and check status:

    >$ systemctl enable ssh
    >$ systemctl status ssh
![](https://i.imgur.com/NQv1cb5.png)
 
 
**IMPORTANT: The client is ssh, the daemon is sshd**

Masking services

Installed services may have conflicting. For example, there are multiple
methods to manage networks and firewalls. To prevent from accidentally starting service, that service can be *masked*.
Mask is a stronger version of *disable*. Using *disable* all symlinks of the specified unit file are removed. If using *mask* the units will be linked to /dev/null

![](https://i.imgur.com/MjiXKeU.png)
 
## Summary
  This short tutorial shows how indetify installed and running serives on the system and manipulate them. 
  
  Summary of **systemctl** commands.
  
| Command | Description |
| ------------- | ------------- |
| **systemctl status UNIT**  | View detailed inforamtion about a unit state.  |
| **systemctl stop UNIT**   | Stop a service on a running system.  |
| **systemctl start UNIT**   | Start a service on a running system.  |
| **systemctl restart UNIT**   | Restart a service on a running system.  |
| **systemctl reload UNIT**   | Reload configuration file of a running service.  |
| **systemctl enable UNIT**   | Enable service to start at boot time.  |
| **systemctl disable UNIT**   | Disable service to start at boot time.  |
