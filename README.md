## Topic
  The text was created for the needs of the II project (Inżynieria internetu).

  Main goal of this tutorial is to control and monitor network services and system daemons using **systemd** in Linux systems.

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

