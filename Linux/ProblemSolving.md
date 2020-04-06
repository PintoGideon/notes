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
#
# NAME
#
#	.bashrc
#
# DESC
#
#	`.bashrc' sets the basic environment for bash shells.
#
#	This specific file sets the prompt, path, and terminal title.
#	Alias and functions are defined separately in the login-specific
#	file, .bash_profile
#
# HISTORY
# 20 June 2000
#  o Initial design and coding
#
# 21 June 2000
#  o Building of separate "informational" functions
#
# 06 January 2000
#  o Merging of old tcsh .cshrc and .login files
#
# 08 January 2000
#  o Need to separate monolithic file into .bashrc and .bash_profile - this
#    is largely for ssh purposes: the splash screen that the monolithic
#    file created confuses ssh.
#
# 25 Septmeber 2002
#  o Removed JAVA_HOME export - seemed to confuse openoffice
#
# 19 February 2003
#  o Began updates for MGH-environment
#
# 04 March 2004
#  o Misc updates and fixing... df usage seems to cause problems on some
#    machines, causing logins to hang
#       - When shell logs in, the prompt is "statsBasic".
#       - From the command line, call "statsPartition" for partition information
#         (based on 'df' and 'du') in the xterm titlebar; or call "statsFull"
#         for partition information *and* load *and* file information in the
#         console prompt itself.
#
# 22 July 2004
#  o Added some NMR aliases
#
##

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

if [[ -f /usr/bin/uname ]] ; then
	arch=$(/usr/bin/uname)
fi

if [[ -f /bin/uname ]] ; then
	arch=$(/bin/uname)
fi

#\\\
# KDE configuration
#///
#export KDEDIR=/usr/kde/3.4

# The following is designed to support custom-built KDE installations
#       that live (typically) somewhere off the $HOME tree. Most of these
#       are legacy leftovers that are kept as placeholders if necessary.
case $HOSTNAME in
	kaos | heisenberg )
		export KDEDIR=/usr
	;;
	nebula | pulsar | bigbird | mongo )
		export KDEDIR="${HOME}"/arch/${arch}/kde3
	;;

	denali )
		export KDEDIR="${HOME}"/arch/${arch}/kde3
	;;

	cambrian.cortechs.net )
		export KDEDIRS=/opt/kde-3.1
		export QTDIR=/opt/qt-3.1.1
		export KDEHOME=~/.kde-3.1
	;;
	reward)
		export KDEHOME=~/.kde-3.2
	;;

	triassic )
		 export KDEDIR=/usr/kde/3.3
		 export KDEDIRS=/usr/kde/3.3
	;;
esac

# Define some colours for prompt usage
       BLACK="\[\033[0;30m\]"
         RED="\[\033[0;31m\]"
       GREEN="\[\033[0;32m\]"
       BROWN="\[\033[0;33m\]"
        BLUE="\[\033[0;34m\]"
      PURPLE="\[\033[0;35m\]"
        CYAN="\[\033[0;36m\]"
  LIGHT_GRAY="\[\033[0;37m\]"

   DARK_GRAY="\[\033[1;30m\]"
   LIGHT_RED="\[\033[1;31m\]"
 LIGHT_GREEN="\[\033[1;32m\]"
      YELLOW="\[\033[1;33m\]"
  LIGHT_BLUE="\[\033[1;34m\]"
LIGHT_PURPLE="\[\033[1;35m\]"
  LIGHT_CYAN="\[\033[1;36m\]"
       WHITE="\[\033[1;37m\]"

 WHITE_BCKGRND="\[\033[47m\]"
  CYAN_BCKGRND="\[\033[46m\]"
PURPLE_BCKGRND="\[\033[45m\]"
  BLUE_BCKGRND="\[\033[44m\]"
 BROWN_BCKGRND="\[\033[43m\]"
 GREEN_BCKGRND="\[\033[42m\]"
   RED_BCKGRND="\[\033[41m\]"
 BLACK_BCKGRND="\[\033[40m\]"

    NO_COLOUR="\[\033[0m\]"


#\\\
# Define some miscellaneous functions
#///

function prompt_command
{
        #
	# DESC
	#       Define a series of commands that are executed each time a new prompt is
	#       generated. Here, the prompt is constructed from the last 15 characters
        #       of the current absolute path
	#

        if [ "$PWD" = "$HOME" ] ; then
                PWD="~"
        fi

	# Shorten the pwd
	pwd_length=15
	if [ $(echo -n $PWD | wc -c | tr -d " ") -gt $pwd_length ]
	then
   		newPWD="${PROMPTPREFIX}...$(echo -n $PWD | sed -e "s/.*\(.\{$pwd_length\}\)/\1/")"
	else
   		newPWD="${PROMPTPREFIX}$(echo -n $PWD)"
	fi
}

# Set the PROMPT_COMMAND macro
PROMPT_COMMAND=prompt_command

function statsBasic {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal - in the xterm window title.
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

	PS1="${TITLEBARBASIC}${GREEN}\${newPWD}\\$>${NO_COLOUR}"
	PS2='> '
	PS4='+ '
}

function TTstatsBasic {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal - in the console display
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

	PS1="${LIGHT_BLUE}${TTBASIC}${LIGHT_GREEN}\${newPWD}\\$>${WHITE}"
	PS2='> '
	PS4='+ '
}

function statsPartition {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

	PS1="${TITLEBARPARTITION}${LIGHT_GREEN}\${newPWD}\\$>${WHITE}"
	PS2='> '
	PS4='+ '
}

function TTstatsPartition {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

	PS1="${LIGHT_BLUE}${TTPARTITION}${LIGHT_GREEN}\${newPWD}\\$>${WHITE}"
	PS2='> '
	PS4='+ '
}

function statsFull {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

        PS1="${TITLEBARBASIC}\n${BLUE_BCKGRND}$WHITE\033[K$(uname -sr) [${HOST_COLOUR}$(currentLoadInfo_determine)${WHITE}][${FILESTATS}][${PARTITIONSTATS}]${NO_COLOUR}\n${LIGHT_GREEN}\${newPWD}>${WHITE}"
	PS2='> '
	PS4='+ '
}

function TTstatsFull {
	#
	# DESC
	#       This function generates a statistics strip at the top of the
	#       terminal
	#
        # HISTORY
        # 03 March 2004
        #  o Split prompt into different functions depending on desired complexity.
        #

        PS1="${LIGHT_BLUE}${TTFULL}${LIGHT_GREEN}\${newPWD}>${WHITE}"
	PS2='> '
	PS4='+ '
}

function currentPartitionUse_determine
{
        #
        # DESC
        #       Determines the density of the current partition as
        #       a current/total string, i.e. 598M/9.6G
        #
        # HISTORY
        # 06 January 2001
        #  o Initial design and coding
        #
        # 03 March 2004
        #  o The use of 'df' here might be problematic in some cases
        #

        partitionUse=$(df -kh . | grep -v system)
        echo $partitionUse | awk '{print $3 "B/" $2 "B"}'
}

function currentDirSize_determine
{
	#
	# DESC
	#       Determines the size of the current directory
	#
        # HISTORY
        # 03 March 2004
        #  o Processing dir size might cause delays in some architectures.
        #

	let totalBytes=0
        let totalSize=0

	for bytes in $(/bin/ls -l | grep "^-" | sed -e "s/ \+/ /g" | awk '{print $5}')
	do
   		let totalBytes=$totalBytes+$bytes
	done

	# The if...fi's give a more specific output in byte, kilobyte, megabyte,
	# and gigabyte

	if [ $totalBytes -lt 1024 ]; then
   		totalSize=$(echo -e "scale=3 \n$totalBytes \nquit" | bc)B
		else if [ $totalBytes -lt 1048576 ]; then
   			totalSize=$(echo -e "scale=3 \n$totalBytes/1024 \nquit" | bc)kB
			else if [ $totalBytes -lt 1073741824 ]; then
   				totalSize=$(echo -e "scale=3 \n$totalBytes/1048576 \nquit" | bc)MB
				else
   					totalSize=$(echo -e "scale=3 \n$totalBytes/1073741824 \nquit" | bc)GB
			fi
		fi
	fi
	echo $totalSize
}

function currentDirFileStats_determine
{
	#
	# DESC
	#       Determine some stats of files in current directory.
	#       More specifically, count the number of file and directories
	#       (hidden and visible), executables, and special block devices
	#

	let files=$(ls -l                 | grep "^-"  | wc -l | tr -d " ")
	let hiddenfiles=$(ls -ld .*       | grep "^-"  | wc -l | tr -d " ")
	let executables=$(ls -l           | grep ^-..x | wc -l | tr -d " ")
	let directories=$(ls -l           | grep "^d"  | wc -l | tr -d " ")
	let hiddendirectories=$(ls -ld .* | grep "^d"  | wc -l | tr -d " ")-2


	let linktemp=$(ls -l              | grep "^l"  | wc -l | tr -d " ")
	if [ "$linktemp" -eq "0" ]; then
    	links=""
	else
    	links=" ${linktemp}"
	fi
	unset linktemp

	let devicetemp=$(ls -l      | grep "^[bc]"| wc -l | tr -d " ")
	if [ "$devicetemp" -eq "0" ]; then
    	devices="0"
	else
    	devices=" ${devicetemp}"
	fi
	unset devicetemp

        echo ${files}\|${hiddenfiles}f ${executables}x ${directories}\|${hiddendirectories}d ${links}l ${devices}v
}

function currentLoadInfo_determine
{
	#
	# DESC
	#       Determine some statistics about the current load
	#       based on the difference between the 1 and 5 minute loads reported
	#       by "uptime"
	#

	if [ -x /usr/bin/uptime ] ; then
		local one=$(uptime | sed -e "s/.*load average: \(.*\...\), \(.*\...\), \(.*\...\)/\1/" -e "s/ //g")
		local five=$(uptime | sed -e "s/.*load average: \(.*\...\), \(.*\...\), \(.*\...\).*/\2/" -e "s/ //g")
		local diff1_5=$(echo -e "scale = scale ($one) \nx=$one - $five\n print \" \"\n print x \nquit \n" | bc)
		loaddiff="$(echo -n "${one}${diff1_5}")"

    	        load=$(echo -e "scale = 0 \n $one/0.01 \nquit \n" | bc)
    	        if [ $load -gt 200 ]; then
    		        HOST_COLOUR=$LIGHT_RED
    		        else if [ $load -gt 100 ]; then
    			        HOST_COLOUR=$YELLOW
    			else
    				HOST_COLOUR=$LIGHT_GREEN
    		        fi
    	        fi
	else
		loaddiff="No Load"
	fi
        echo $loaddiff
#	return $loaddiff
}

function dsend
	#
	# ARGS
	#	$1		text string to send
	#
	# DESC
	# 	A simple wrapper around 'SSocket_client'.
	#
	# HISTORY
	# 09 March 2005
	#  o Initial design and coding.
	#
{
	SSocket_client --msg "$1"
}

function manpr
        #
        # ARGS
        #       $1            man page to print
        #       $2            destination device. "-" is stdout
        #
        # DESC
        #       If $2 == "-", outputs PostScript to stdout, else assumes
	#       that $2 denotes a printer name
	#
        # HISTORY
        # 09 January 2001
        #  o Initial design and coding
        #
{
	case $2 in
		- )
        	man $1 | nroff -man | enscript -1RjG -fCourier8 -p -
		;;
		* )
		man $1 | nroff -man | enscript -1RjG -fCourier8 -P $2
	esac
}

function manmac
{
	man -t $1 | open -f -a preview
}

function cdr
{
	cd /cygdrive/$1
}

function dkf
{
        NAME=$1
        ID=$(dkl | grep $NAME | head -n 1 | awk '{print $1}')
        docker logs --follow $ID
}

function dkla
{
        NAME=$1
        docker ps -a --no-trunc | grep $NAME
}

function dkrm
{
        NAME=$1
        ID=$(dkl | grep $NAME | head -n 1 | awk '{print $1}')
        docker stop $ID && docker rm -vf $ID
}

function dke {
        NAME=$1
        ID=$(dkl | grep $NAME | head -n 1 | awk '{print $1}')
        docker exec -ti $ID /bin/bash
}

function dkcp {
	LOCALDIR=$1
	NAME=$2
	CONTDIR=$3
        ID=$(dkl | grep $NAME | head -n 1 | awk '{print $1}')
	docker cp $LOCALDIR ${ID}:${CONTDIR}
}

#\\\
# Define the xterm title bar
#///
TTY=$(tty | sed 's/\/dev\///')
# Simplest case, for all shells / architectures
TITLEBARBASIC='\[\033]0;\u@\h ($OSTYPE) $TTY \w\007\]'
TTBASIC='\033[s\033[0;0H\033[K\u@\h ($OSTYPE) $TTY \w\007\033[u\]'
case $TERM in
	*term* | *win* | *rxvt* )
	if [ $OSTYPE = beos ] ; then
                TITLEBARBASIC='\[\033]0;\u ($OSTYPE) $TTY \w\007\]'
	else
                TITLEBARPARTITION='\[\033]0;\u@\h ($OSTYPE) [$(currentDirSize_determine)/$(currentPartitionUse_determine)] $TTY \w\007\]'
                TTPARTITION='\033[s\033[0;0H\033[K\u@\h ($OSTYPE) [$(currentDirSize_determine)/$(currentPartitionUse_determine)] $TTY \w\007\033[u\]'
                PARTITIONSTATS='$(currentDirSize_determine)/$(currentPartitionUse_determine)'
                FILESTATS='$(currentDirFileStats_determine)'
                LOADSTATS='$(currentLoadInfo_determine)'
                TTFULL='\033[s\033[0;0H\033[K\u@\h $(uname -sr) [$(currentLoadInfo_determine)][$(currentDirFileStats_determine)][$(currentDirSize_determine)/$(currentPartitionUse_determine)] $TTY \w\007\033[u\]'
        fi
        ;;
	*)
                TITLEBARBASIC=""
        ;;
esac

#\\\
# Path concerns
#///
name=$(hostname | awk -F. '{print $1}')

# The "self" directory is simply the home directory on the local machine.
# It is assumed that the "arch" and "arch/scripts" directories on the
# home partition are either local or linked to the correct partitions.
self=$HOME

# The "lab" directory denotes a "group" shared directory.
case $name in
        "pangea" )      lab=/opt			;;
        "localhost" )   lab=/opt			;;
        * )             lab=/opt			;;
esac

# Setup some variables for local/self/lab/java paths
b_64=$(uname -a | grep x86_64 | wc -l)
if (( b_64 )) ; then
	BIT=64
else
	BIT=32
fi

for DIR in bin lib scripts ; do
        for ACCESS in local self lab java python npm ; do
        	arch=$(uname)
		b_NT=$(uname -a | grep CYGWIN | wc -l)
		if (( b_NT )) ; then
			arch=win
		fi
                case $ACCESS in
                        "self" )        root=$self      ;;
                        "lab" )         root=$lab       ;;
                        "java" )        root=$self
                                        arch="java"     ;;
			"npm" )		root=$self
					arch="npm"
					BIT=""		;;
			"python" )	root=$self
					arch="python"
					BIT=""		;;
 			"local" )	root=$self
					arch="local"	;;
                esac
                if [ $DIR = "scripts" ] ; then
                        export "${ACCESS}_${DIR}"="${root}/arch/${DIR}"
                else
                        export "${ACCESS}_${DIR}"="${root}/arch/${arch}${BIT}/${DIR}"
                fi
        done
done
arch=$(uname)

statsBasic
if [ -f ~/.ls_colours_bash ] ; then
        . ~/.ls_colours_bash
fi

#\\\
# Some internal shell variables
#///
umask u=rwx,g=rwx,o=rx
set     -o ignoreeof
set     -o emacs
if [ $arch != BeOS ] ; then
	set     -o notify
fi
shopt   -s cdspell
shopt   -s checkwinsize

#\\
# Miscellaneous environment variables
#///
export CLASSPATH=$java_lib
export SAL_IGNOREXERRORS=1
export PRINTER=HPhpLaserJet1000
export FONT=7x14bold
export TAPE=/dev/nrtape/dat
export ENSCRIPT="-1RjG -P$PRINTER -MLetter -fHelvetica10"
export ORGANIZATION="MGH/MIT/HMS Athinoula A. Martinos Center for Biomedical Imaging"
export REPLYTO=$(whoami)@$(hostname)
#export EMACSPACKAGEPATH="${HOME}"/arch/common/share/xemacs21/packages
export NNTPSERVER=news
export COLORTERM=1
export SDKEYDIR=${lab}/arch/common/sdfast
export PSTILL_PATH=${lab}/arch/${arch}/pstill_dist
#export FILEWATCH="--optargs -p__/home/rudolph/arch/scripts/System_sounds/__-f__#file__-c__%pid"
export FLASH_GTK_LIBRARY=libgtk-x11-2.0.so.0
export PYTHONPATH=~/arch/python/lib/python2.7/site-packages

#\\
# CVS related environment variables
#//

case $HOSTNAME in
	mosix* )
		export CVSROOT=:pserver:${USER}@pangea:/mch/mch3/proj/CVS_repository
	;;
	*localhost* )
		export CVSROOT=:pserver:${USER}@pangea:/mch/mch3/proj/CVS_repository
	;;
	* )
		#export CVSROOT=:ext:rudolph@gate.nmr.mgh.harvard.edu:/space/repo/1/dev
		export CVSROOT=:ext:rudolph@gate.nmr.mgh.harvard.edu:/space/repo/1/sanfs
		export CVS_RSH=ssh
	;;
esac

#\\\
# Miscellaneous aliases
#///
alias xsu='xterm -bg "#440000" -fg white -fn $FONT -T root@$(hostname) -e su -'
alias htv='xterm -g 80x60 -e lynx'
alias xless='xless -g 80x45'
alias rmi='rm -i'
alias skiet='kill -9'
alias bye='logout'
alias clean='rm -f $(/bin/ls *% .*% .*~ *~ \#*\# [c][o][r][e] [L][O][G] 2>/dev/null | tee /dev/tty) >/dev/null'
alias c='clear'
alias qtld='export LD_LIBRARY_PATH=/home/pienaar/arch/${arch}/qt/lib:$LD_LIBRARY_PATH'
alias psap='ps -Af | grep '

#\\\
# other aliases
#///
alias ssc='source "${HOME}"/.bashrc ; source "${HOME}"/.bash_profile ; source "${HOME}"/.ls_colours_bash-bgblack'
alias nse='source "${HOME}"/arch/scripts/nse'
alias cnde='source "${HOME}"/arch/scripts/cnde'
alias neuro-fsdev='source "${HOME}"/arch/scripts/neuro-fs dev'
alias neuro-fsstable='source "${HOME}"/arch/scripts/neuro-fs stable'
alias neuro-fsunstable='source "${HOME}"/arch/scripts/neuro-fs unstable'
alias neuro-connectome='source "${HOME}"/arch/scripts/neuro-connectome'
alias neuro-connectome-latest='source "${HOME}"/arch/scripts/neuro-connectome-latest'
alias neuro-connectome-dbg='source "${HOME}"/arch/scripts/neuro-connectome-dbg'
alias lnde='source "${HOME}"/arch/scripts/lnde'
alias nde='source "${HOME}"/arch/scripts/nde'
alias lde='source "${HOME}"/arch/scripts/lde'
alias rde='source "${HOME}"/arch/scripts/rde'
alias rde64='source "${HOME}"/arch/scripts/rde64'
alias rde64-dev='source "${HOME}"/arch/scripts/rde64-dev'
alias mde='source "${HOME}"/arch/scripts/mde'
alias nsed='source "${HOME}"/arch/scripts/nsed'
alias ss='source "${HOME}"/.bashrc ; source "${HOME}"/.bash_profile'
#alias m='/space/lyon/9/pubsw/Linux2/common/matlab/7.0.1/bin/matlab -nosplash -nodesktop'
alias m='~/arch/scripts/matlab -c'
alias plm='LD_PRELOAD=/lib/libgcc_s.so.1 /space/lyon/9/pubsw/Linux2-2.3-i386.new/bin/matlab7.7 -nosplash -nodesktop'
alias mu='MATLAB_JAVA=/usr/lib/jvm/java-6-sun/jre /space/lyon/9/pubsw/Linux2-2.3-i386.new/bin/matlab7.7 -nosplash -nodesktop'
alias m64='/space/lyon/9/pubsw/Linux2-2.3-x86_64/bin/matlab7.8 -nosplash -nodesktop'
alias mmac='/usr/pubsw/packages/matlab/7.8/bin/matlab -nosplash -nodesktop'
alias mneuro='matlab -nosplash -nodesktop'
alias es='export SUBJECTS_DIR=$(pwd)'
alias ep='export PATH=$(pwd):$PATH'
alias ks='pushd . >/dev/null ; cd /cmas/fs/1/users/freesurfer/Subjects/surf_parc/kentron ; es ; popd >/dev/null'
alias uldp='unset LD_LIBRARY_PATH'
alias aldp='export LD_LIBRARY_PATH=$(pwd):$LD_LIBRARY_PATH'
alias cms='pushd . >/dev/null ; cd /cmas/fs/1/users/freesurfer/Subjects ; es ; popd >/dev/null'
alias oo='"${HOME}"/arch/Linux/packages/OpenOffice.org/soffice'
alias oob='"${HOME}"/arch/Linux/packages/OpenOffice.org/soffice "-accept=socket,host=localhost,port=2002;urp;"'
alias gp='export XDG_CONFIG_DIRS=/opt/local/etc/xdg ; export XDG_DATA_DIRS=/opt/local/share'
alias ']'='gnome-open'
alias dks="docker ps -a | grep -v CONT | awk '{printf(\"docker stop %s && docker rm -vf %s\n\", \$1, \$1);}' | sh -v"
alias dkl="docker ps -a"
alias dsl="docker service ls"
alias dss="docker service rm "
alias dtag="pfdicom_tagExtract -I ./ -O /tmp --printToScreen -i "

export bashrc_read=1

if [[ -f "${HOME}"/arch/scripts/localEnv.bash ]] ; then
	source "${HOME}"/arch/scripts/localEnv.bash
fi



# This line was appended by KDE
# Make sure our customised gtkrc file is loaded.
export GTK2_RC_FILES=$HOME/.gtkrc-2.0
export OOO_FORCE_DESKTOP=gnome


# added by Anaconda3 installer
export PATH="/home/rudolph/arch-local/x86_64-Linux/packages/anaconda3/bin:$PATH"

export NVM_DIR="$HOME/.nvm"
if [[ -s "$NVM_DIR/nvm.sh" ]] ; then
    cd $NVM_DIR ;
    source "./nvm.sh"  # This loads nvm
    cd ~
fi
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion



```

### Bash Profle

```sh
##
#
# NAME
#
#	.bash_profile
#
# DESC
#
#	`.bash_profile' is the bash analogue of .login
#
# HISTORY
#
# 08 January 2000
#  o Separation from monolithic .bashrc.
#  o Contains basically only the splash screen
#
# 15 March 2002
#  o Expanded $PATH for gcc-3 "separate" installation tree.
#
# 03 March 2004
#  o Edits to CYGWIN path
#
# 08 June 2005
#  o Added 'Darwin' section
#
# 25 October 2011
#  o git additions...
##

if [[ -f /usr/bin/uname ]] ; then
	arch=$(/usr/bin/uname)
fi

if [[ -f /bin/uname ]] ; then
	arch=$(/bin/uname)
fi

if [ ! ${bashrc_read:+1} ] ; then
        . ~/.bashrc
        export bash_profile_read=1
fi

if [ $arch = BeOS ] ; then
	HOSTNAME=BeOS
fi

function git_current {
   BRANCH_NAME=`git branch -v 2>/dev/null |grep '^*'|awk '{print $2}'`
   BRANCH_HASH=`git branch --abbrev=4 -v 2>/dev/null |grep '^*'|grep -o '[0-9a-f][0-9a-f][0-9a-f][0-9a-f]' | tr '\n' '-'`
   if (( ${#BRANCH_NAME} )) ; then
       echo -n "($BRANCH_NAME@$BRANCH_HASH)"
   fi
}

if [[ -f ~/.git.bash ]] ; then
    GIT_PROMPT='$(git_current)'
    if [[ $GIT_READ != "1" ]] ; then
	PS1orig="$PS1"
    fi
#    PS1_noTrail=$(echo $PS1orig | awk -F '\\$>' '{printf("%s\b%s\n", $1, $2);}')
#    PS1="${PS1_noTrail}${BROWN}${GIT_PROMPT}${NO_COLOUR}\\$>"

    PS1="${YELLOW}${TITLEBARBASIC}${GREEN}\${newPWD}${BROWN}${GIT_PROMPT}${NO_COLOUR}\\$>"
    PS2='> '
    PS4='+ '

    source ~/.git.bash
    GIT_READ=1
fi

#\\\
# DISPLAY variable
#///
if [ $TERM = "xterm" ] ; then
        if [ ! ${DISPLAY:+1} ] ; then
                echo -n "X display name: "
                read display
                export DISPLAY=${display}:0.0
        fi
fi

#\\\
# Draw splash screen
#///
export http_proxy=http://proxy.tch.harvard.edu:3128
echo -en "\033[0m"
alias sep="echo +-----------------------------------------------------------------------------+"
sep
echo -en "\t\t\tWelcome to "
echo -e "\033[45m\033[1;33m$HOSTNAME\033[0m, $USER"
sep
echo -e "Terminal is an $TERM, on a $arch architecture\033[1;31m"
/usr/bin/uptime 2>/dev/null
echo -en "\033[0m"
sep
echo -en "\033[0;36m"

#\\\
# Some platform specific issues
#///
case $arch in
        Linux )

                #export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$java_bin:${QTDIR}/bin:${KDEDIRS}/bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/games:/etc:/usr/etc:/usr/bin/X11:/usr/man:/usr/X11R6/bin:/opt/gnome/bin:/cmas/nodulus/1/common/Linux/bin"
                export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin:${QTDIR}/bin:${KDEDIRS}/bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/games:/etc:/usr/etc:/usr/bin/X11:/usr/man:/usr/X11R6/bin:/opt/gnome/bin:/opt/torque/bin"
                alias cp='cp -pvrdi'
                alias ls='ls -CFh --color'
                mesg y
                finger -s
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
        FreeBSD )
                export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/games:/etc:/usr/openwin/bin:/usr/etc:/usr/bin/X11:/usr/man:/usr/X11R6/bin"
                alias cp='cp -pPRi'
                alias ls='ls -CFG'
		export LSCOLORS="DxGxcxdxCxegedabagacad"
                finger
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
        IRIX | IRIX64 )
                alias wavplay='soundplayer -nodisplay'
                alias play='/usr/sbin/soundplayer -nodisplay'
		alias xterm=color_xterm
		alias ping='ping -c 4'
                export LD_LIBRARY_PATH=/usr/freeware/lib:${LD_LIBRARY_PATH}
                export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin:${KDEDIR}/bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/bsd:/etc:/usr/etc:/usr/bin/X11:/usr/man:/usr/local/sdfast/bin:/usr/freeware/bin:/usr/local/freeware/bin:/usr/local_bme/bin:/usr/java/bin"
                if [ -x /usr/freeware/bin/cp ] ; then
                        alias cp='/usr/freeware/bin/cp -pvrdi'
                fi
		alias ls='ls -CFh --color'
                mesg y
                finger -s
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
        BeOS )
                alias cp='cp -pvrdi'
                alias ls='ls -CFh --color'
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
        SunOS )
                export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/games:/etc:/usr/openwin/bin:/usr/etc:/usr/man:/opt/sfw/bin"
                alias cp='cp -pvrdi'
                alias ls='ls -CFh --color'
                mesg y
                finger -s
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
        CYGWIN* )
		export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin::${PATH}"
                alias cp='cp -pvrdi'
                alias ls='ls -CFh --color'
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
        ;;
       Darwin )
                export PATH=".:$local_bin:$self_bin:$self_scripts:$lab_bin:$lab_scripts:$python_bin:$npm_bin:$java_bin:${QTDIR}/bin:${KDEDIRS}/bin:/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/games:/etc:/usr/etc:/usr/bin/X11:/usr/man:/usr/X11R6/bin:/opt/gnome/bin:/opt/local/bin:/sw/bin:/opt/arch/x86_64-Darwin/bin"
                alias cp='gcp -pvrdi'
		if [[ -x gls ]] ; then
		    alias ls='gls -CFh --color'
		else
                    alias ls='ls -CFhG'
	        fi
		export TERM=xterm-color
		export CLICOLOR=1
		export CLICOLOR_FORCE=1
		export LSCOLORS="DxGxcxdxCxegedabagacad"
                mesg y
                finger -s
		echo -en "\033[0m"
		sep
		echo -en "\033[1;35m"
		fortune
		echo -en "\033[0m"
	;;
        * )
                mesg y
                who
                echo $sep
                echo -en "\033[0;31m"
                echo "( I cannot identify this architecture )"
        ;;
esac
echo -en "\033[0m"
sep
unalias sep
now
if [[ $(type -P screenfetch) ]] ; then
	echo ""
	screenfetch 2>/dev/null
	echo ""
fi

##
# Your previous /Users/rudolphpienaar/.bash_profile file was backed up as /Users/rudolphpienaar/.bash_profile.macports-saved_2009-09-10_at_13:29:49
##

# MacPorts Installer addition on 2009-09-10_at_13:29:49: adding an appropriate PATH variable for use with MacPorts.
export PATH=/opt/local/bin:/opt/local/sbin:$PATH
# Finished adapting your PATH environment variable for use with MacPorts.


##
# Your previous /Users/rudolphpienaar/.bash_profile file was backed up as /Users/rudolphpienaar/.bash_profile.macports-saved_2009-09-11_at_09:30:28
##

# MacPorts Installer addition on 2009-09-11_at_09:30:28: adding an appropriate PATH variable for use with MacPorts.
export PATH=/opt/local/bin:/opt/local/sbin:$PATH
# Finished adapting your PATH environment variable for use with MacPorts.
```
