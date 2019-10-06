"Over the last few days there has not been an area to input a MRN number for pacs_pull, or any other plugin for that matter. I would really appreciate help in resolving this matter as there are very limited things for me to do without ChRIS"

```bash

$> id gideon.pinto
id:'gideon.pinto': no such user

$> ssh root@fnndsc

root@fnndsc's password: Toor's password

$> cd /etc/init.d
$> ls
$> ./sladp restart


```

### In a different terminal

```bash

$>id gideon.pinto
$>ls
$> cd src/
$> ls
$> cd chrisreloaded
$> ls
$> cd tools
$> ls
$> more use.txt
$> checkDB.bash -h /neuro/chris/users -n chris -u chris

```

```bash
cd /etc/init.d/
```

### How to create a user account for a lab member

```bash

ssh root@fnndsc
ssc
history | grep neuro
494 neuro-adduser.sh -u mohammed.fouda -g grantlab,chrisgp
ssh mohammed.fouda@rc-drno
neuro-adduser.sh -x
neuro-adduser.sh -u mohammed.fouda -G nirsgp


```

### How to fix the login issue

```bash

Press Crtl + Alt +F7 to switch to an text interface
Try username@computername
Alt F7 to switch out
apt install ssdm
apt install kubuntu-desktop
Logout and log back in.

```

```bash

The path /python3 (from --python=/python3) does not exist
```

**_ Cannot connect to chris website _**

```bash
ssh toor@chris-tmp
sudo service apache2 restart
```
