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

$> ssh chris@chris-tmp
$> ls
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
```

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

```sh

./px-push                                                     \
                   --swiftIP 0.0.0.0                                \
                   --swiftPort 8080                                        \
                   --swiftLogin chris:chris1234                            \
                   --swiftServicesPACS covidnet                            \
                   --swiftPackEachDICOM                                    \
                   --xcrdir /home/gideon/covidnet_integration/images       \
                   --parseAllFilesWithSubStr dcm                           \
                   --verbosity 1                                           \
                   --json > push.json



```

```sh

./px-register                                                 \
                       --upstreamFile push.json                            \
                       --CUBEURL http://localhost:8333/api/v1/             \
                       --CUBEusername chris                                \
                       --CUBEuserpasswd chris1234                          \
                       --swiftServicesPACS covidnet                        \
                       --verbosity 1                                       \
                       --json                                              \
                       --logdir /tmp/log                                   \
                       --debug
```





### Feedback for Alisamar

When you select a search criteria from the dropdown, there is no indicator to show 
which option is selected.

```jsx
<Button isLarge variant='primary' id='finalize' onClick={finalize}
icon={<SearchIcon/>}
>Search</Button>

```

We have a couple of invalid Dom nesting warnings.
When the query builder returns a single patient, it should be 1 patient matched your search.

Similarly, if you have a single study, it should say 1 study.





```sh
curl -X 'PUT' \
  'http://localhost:4005/api/v1/PACSservice/orthanc' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "info": {
    "aet": "CHRISLOCAL",
    "aet_listener": "ORTHANC",
    "aec": "ORTHANC",
    "serverIP":"192.168.1.109",
    "serverPort": "4242"
  }
}' | jq

```


```sh
curl -X 'POST' \
  'http://localhost:4005/api/v1/PACS/pypx/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "PACSservice": {
    "value": "orthanc"
  },
  "listenerService": {
    "value": "default"
  },
  "pypx_find": {
    "AccessionNumber": "",
    "PatientID": "1449c1d",
    "PatientName": "",
    "PatientBirthDate": "",
    "PatientAge": "",
    "PatientSex": "",
    "StudyDate": "",
    "StudyDescription": "",
    "StudyInstanceUID": "",
    "Modality": "",
    "ModalitiesInStudy": "",
    "PerformedStationAETitle": "",
    "NumberOfSeriesRelatedInstances": "",
    "InstanceNumber": "",
    "SeriesDate": "",
    "SeriesDescription": "",
    "SeriesInstanceUID": "",
    "ProtocolName": "",
    "AcquisitionProtocolDescription": "",
    "AcquisitionProtocolName": "",
    "withFeedBack": false,
    "then": "",
    "dblogbasepath": "/home/dicom/log",
    "json_response": true
  }
}'


```


```javascript

1- git commit and push modified files after successful yarn test
2- increase version in package.json and yarn build the package
3- npm publish
4- git commit and push package.json and built package with -m "v.0.0.3"  format

```

You need to switch to https://github.com/FNNDSC/fnndsc/tree/gh-pages branch
But first in  master  do

```
yarn docs
mv doc/ /<dir_containing_gh-pages>/chrisdoc/
```
And push the changes to rebuild the docs



### Cube- pdcm

```md
REACT_APP_PFDCM_URL=https://pfdcm.pangea.chrisproject.org
```
Minimize the footprint in each component
Sequential matching of the routes takes a while to render

```
https://meet.google.com/dhb-rfcy-rzz
```


```

titan
moc
bu-21-9
ulsan

```


```javascript

 date.setUTCFullYear(parseInt(value.slice(0, 4)));

```

### Personal Access Token

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





