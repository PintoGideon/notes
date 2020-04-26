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

$>ssh chris@chris-tmp
$>ls
$> cd src/
$> ls
$> cd chrisreloaded
$> ls
$> cd tools
$> ls
$> more use.txt
$> checkDB.bash -h /neuro/users/chris/users -n chris -u chris

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

### How to fix the login issue in the display manager

```bash

Press Crtl + Alt +F7 to switch to an text interface
Try username@computername
sudo dpkg-reconfigure ssdm
sudo service ssdm restart

Alt F7 to switch out
apt install ssdm
apt install kubuntu-desktop
Logout and log back in.

```

**_ Cannot connect to chris website _**

```bash
ssh toor@chris-tmp
sudo service apache2 restart
```

### Setting up ChRIS instances on fnndsc

```bash
cd /home/jorge.bernal/chris/ChRIS_ultron_backEnd
./docker-destroy-chris_dev.sh
sudo rm -r FS/
git pull
./docker-make-chris_dev.sh -U -I -i
cd /home/jorge.bernal/chris/ChRIS_store_ui
git pull
docker build -t local/chris_store_ui .
docker run --name chris_store_ui -p 5055:3000 -d local/chris_store_ui
cd /home/jorge.bernal/chris/ChRIS_ui
git pull
docker build -t local/chris_ui .
docker run --name chris_ui -p 5010:3000 -d local/chris_ui
```

### Scripts to push files to the swift storage.

```bash
./pushAllToCUBE.sh -E dcm -D $DICOMDIR -P DICOM/dataset1 -C $HOST_IP -p $HOST_PORT

```

### Pacs Pull doesn't seem to work on ChRIS

Sometimes the PACS is slow. We can also restart the listener on Pretoria. The service on Pretoria is usually down.

```bash
nc pretoria 10401

```

This check if the listener is actually up. If the above command "hangs", it means the listener is up. If it immediately returns, it means the listener is down.

```bash
ssh toor@pretoria
sudo bash
service xinetd restart
```

### Books to read

```bash
/neuro/labs/grantlab/research/Books/Linux
```

### Download files on the system

```bash
su - toor
sudo dkpg -i whateverfile.deb
```

docker-compose -f docker-compose_dev.yml exec chris_dev python plugins/services/manager.py add "${dockerhubRepo}" $computeEnv http://chrisstore:8010/api/v1/

cd utils/scripts
./pushAllToCUBE.sh -E dcm -D \$DICOMDIR -P validation data

### neuro-adduser.sh

neuro-adduser.sh' is a general FNNDSC LDAP user admin script.

Here are some of the arguments.

```bash

- u <username>
-U <uid>
-g <groupname>

neuro-adduser.sh -u  -G
uid=2249509(ch212561) gid=2000513(domain_users) groups=2000513(domain_users),2202204(rc_hpc)
```

### "No directory, loggin in with HOME"

```bash

ssh root@pretoria
nano /etc/resolv.conf
search tch.harvard.edu chboston.org
```

### Pacs pull parameters

```sh
cd /home/jorge.bernal/chris/ChRIS_ultron_backEnd
./docker-deploy.sh down
git pull
./docker-deploy.sh up
cd /home/jorge.bernal/chris/ChRIS_store_ui
git pull
docker build -t local/chris_store_ui .
docker run --name chris_store_ui -p 5055:3000 -d local/chris_store_ui
cd /home/jorge.bernal/chris/ChRIS_ui
git pull
docker build -t local/chris_ui .
docker run --name chris_ui -p 5010:3000 -d local/chris_ui
```
