<div align="center">
  <img src="../_img/eml_logo_and_text.png">
</div>

# Task Spooler Manual for EDA-Server

## Connection to EDA-Servern:

I refere to [https://phabricator.ict.tuwien.ac.at/w/ict_servicesandinfrastructure/ict_eda_server/]  there you can find all necessary information to connect to the EDA Servers.

You should use one of those two servers

  1. eda01.ict.tuwien.ac.at (128.131.80.56) - 2 GPUs Nvidia V100 with 16GB memory each
  2. eda02.ict.tuwien.ac.at (128.131.80.57) - 2 GPUs Nvidia V100 with 32GB memory each



## Usage of Task-Spooler

On EDA01 you have to use tsp as command, on EDA02 you have to use ts.
Official Documentation can be found here [http://manpages.ubuntu.com/manpages/xenial/man1/tsp.1.html]


### Start of Task-Spooler

You have to export the right socket so your program is in the queue.
1a. should be used for jobs which uses the GPU 1b should be used for jobs without GPU.
You can also set a path were the outputfile of your script will be written into. You can use the predefined outputfile or change the PATH-OUTPUTFILE to your needs.

You must use CHMOD 777 to give every user the permission to use the socket. EVERY Time you export TS_SOCKET.

```
1a. export TS_SOCKET="/srv/ts_socket/CPU.socket"
1b. export TS_SOCKET="/srv/ts_socket/GPU.socket"
2. tsp
3a. chmod 777 /srv/ts_socket/CPU.socket
3b.chmod 777 /srv/ts_socket/GPU.socket
4a. export TS_TMPDIR="/srv/ts_socket/Output/" (Predefined Path for Outputfile)
4b. export TS_TMPDIR="/PATH-OUTPUTFILE.out" (Userpath optional)

```
### Important:
You must use CHMOD 777 to give every user the permission to use the socket. EVERY Time you export TS_SOCKET.

The export command has to be executed outside of an virtual enviroment (pip-enviroment).


### Start of a Script
```
tsp  -L LABEL-FOR-SCRIPT /PATH-TO-SCRIPT
```


### Progress
```
tsp
oder tsp -l
```
### Show the outputfile for <id>
```
tsp -c <id>
```

if the process is still running, you can cancel the outputfile with str+c.



### Changing Jobs in Queue position
```
tsp -U <id-id>
```

### Killing of a running job:
```
kill $(tsp -p <id>)
```
### Removing a not running job from queue:
```
tsp -r <id>
```

### Removing finished jobs from queue:
```
tsp -C
```

### Sending Datafiles to/from the server:

Sending to the server:
```
scp Quelldatei.bsp Benutzer@Host:Verzeichnis/Zieldatei.bsp
```
Receiving from the server:
```
scp Benutzer@Host:Verzeichnis/Quelldatei.bsp Zieldatei.bsp
```

## Examples:

In Path /srv/ts_socket/Testrun lies an example script. (Workscript.sh)

Example 1
```
ssh USERNAME@eda01.ict.tuwien.ac.at
export TS_SOCKET="/srv/ts_socket/CPU.socket"
export TS_TMPDIR="/srv/ts_socket/Output/"
tsp
chmod 777 /srv/ts_socket/CPU.socket
tsp -L "USERNAME" /srv/ts_socket/Testrun/Workscript.sh

```

Example 2
```
#!/bin/bash

echo "Start script with . /[PATH_TO_SCRIPT], not with ./"

NAME=XXX

echo "=== Init task spooler ==="
echo "Setup task spooler socket for GPU."

export TS_SOCKET="/srv/ts_socket/GPU.socket"
chmod 777 /srv/ts_socket/GPU.socket
export TS_TMPDIR=/home/$NAME/logs
echo task spooler output directory: /home/$NAME/logs

```

## Recommended Filestructure:


For every user there you should generate a path like /srv/cdl-eml/User/USERNAME with your USERNAME.
In this path you should make your virtual enviroment.



