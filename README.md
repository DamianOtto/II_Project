## Topic
  The text was created for the needs of the II project (Inżynieria internetu).

  Main goal of this tutorial is to control and monitor network services and system daemons using **systemd** in Linux systems.

## Step 1
  Get linux system, no matter which.

## Step 2 - service states
  The status of a service can be viewed with command **systemctl**

## Step 2 - preparation
  Get example of service.
  Linux has a lot of these but we try to get specific one (and useful!)
  Please, install SSH server and make base configuration:

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
