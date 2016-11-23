# build_debian_arm

This is a simple script to help you to build a debian for your android smartphone/tablet 

I hope it will help people to understant how debian work on arm 

This script will build the debian jessie distribution which will will run on the android kernel. 
This script doesn't recompile the kernel (for now :) )

The first script need to be run on your computer and will build a tar.bz2 file 

You will have to copie this file to your android device 


```shell
#!/bin/sh

#### Build debian arm ####

GREEN='\033[0;32m'
RED='\033[0;31m'
NC='\033[0m' # No Color

FILE=$(pwd)/debian_armhf_jessie

echo "
${GREEN}
###############################################################################

Building debian for armhf processor

###############################################################################
${NC}
"

if [ -d $FILE ]; then
    echo "removing old file"
    rm -rf $FILE
fi


mkdir $FILE


echo "
${GREEN}
##############################################################################################

Beginning of deboostrap

##############################################################################################
${NC}
"

debootstrap --arch=armhf --foreign  jessie  $FILE  http://httpredir.debian.org/debian

echo "
${GREEN}
##############################################################################################

Adding qemu binnary for deboostrap second stage

##############################################################################################
${NC}
"


cp /usr/bin/qemu-arm-static $FILE/usr/bin

echo "
${GREEN}
##############################################################################################

Starting deboostrap second stage

##############################################################################################
${NC}
"

chroot $FILE /debootstrap/debootstrap --second-stage

echo "
${GREEN}
##############################################################################################

Creating tar.bz2 achive

##############################################################################################
${NC}
"

tar jcvf debian_armhf_jessie.tar.bz2 $FILE

echo "
${GREEN}
##############################################################################################

Delete tmp file

##############################################################################################
${NC}
"

rm -rf $FILE

echo "
${GREEN}
##############################################################################################

DONE !!

##############################################################################################
${NC}
"
```
