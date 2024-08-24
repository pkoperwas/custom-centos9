#Download files:

1. Downoload ks.cfg from this repo to /root/ks.cfg
2. Download 11GB http://mirroronet.pl/pub/mirrors/centos-stream/9-stream/BaseOS/x86_64/iso/CentOS-Stream-9-latest-x86_64-dvd1.iso to /root/CentOS-Stream-9-latest-x86_64-dvd1.iso

#Prepare custom ISO on your linux
Copy ISO Contents to Temporary Directory
mount -o loop /root/CentOS-Stream-9-latest-x86_64-dvd1.iso /mnt/iso
shopt -s dotglob
mkdir /tmp/iso
cp -pvRf /mnt/iso/* /tmp/iso
umount /mnt/iso

#Make a Note of the ISO’s Label
blkid /root/CentOS-Stream-9-latest-x86_64-dvd1.iso

#Copy Kickstart File Into ISO Directory
cp /root/ks.cfg /tmp/iso

#Check isolinux Menu for Linux Install Entry
grep -A4 "^label linux" /tmp/iso/isolinux/isolinux.cfg

#Modify the isolinux.cfg file to Point to the New Kickstart File
sed -i '/^\s*append initrd=/ s/$/ inst.ks=cdrom:\/ks.cfg/' /tmp/iso/isolinux/isolinux.cfg
