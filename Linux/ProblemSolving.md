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

When I sign in the plugin section is not displaying the text boxes to
input the information in order to search a patient or perform a
function.
All that is visible is the function image and the rest of the plugin window is blank.

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

### How to create a user account for a lab member

```bash

ssh ch212561@e2
id ch212561
# A unix user id is found
uid=2279731
cd

ssh root@fnndsc
sudo bash
ssc
history | grep neuro
494 neuro-adduser.sh -u mohammed.fouda -U <user.id> -g grantlab,chrisgp
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

This checks if the listener is actually up. If the above command "hangs", it means the listener is up. If it immediately returns, it means the listener is down.

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

### neuro-adduser.sh

`neuro-adduser.sh` is a general FNNDSC LDAP user admin script.

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

```bash
ssh -f -N -L localhost:5902:localhost:5902 -J london titan
vncviewer localhost:5902

```

### Pulse Secure over Ubuntu 20.04

```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/pulse/extra/usr/lib/x86_64-linux-gnu/
```

./plugin_add.sh 
./plugin_add.sh fnndsc/pl-pfdorun_imgmagick -j

```bash
1- Build local pfcon
git clone https://github.com/FNNDSC/pfcon.git
cd pfcon
git checkout flask
docker build -t local/pfcon .


2- Build local CUBE

cd ChRIS_ultron_backEnd
git checkout pfcon_flask
docker build -t local/chris:dev -f Dockerfile_dev .


3- Run CUBE
In docker-compose_dev.yml change CHRISREPO and PFCONREPO to local

./make.sh


http -a cube:cube1234 http://localhost:8000/api/v1/plugins/instances/2/


docker-compose -f docker-compose_dev.yml exec pfcon_service cat /tmp/debug.log
````


```md

COVIDNET_ui is served from-the-metal by the apache2 web server on this host.

The React-TypeScript bundle is built on a foreign machine, then pushed here.

e.g.

    git clone git@github.com:FNNDSC/covidnet_ui.git
    cd covidnet_ui
    git checkout fnndsc-deploy
    npm i

    git pull
    npm run build

    ssh jorge.bernal@fnndsc.childrens.harvard.edu rm -r /var/www/app.chrisproject.org/build
    

    scp -r build jorge.bernal@fnndsc.childrens.harvard.edu:/var/www/covidnet_ui/build
    scp -r build jorge.bernal@fnndsc.childrens.harvard.edu:/var/www/app.chrisproject.org/build

```




