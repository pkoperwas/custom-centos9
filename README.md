# How to prepare own custom (via kickstart) Centos9 ISO file 
#
*steps tested on centos9*
#


**Download files:**
```
wget https://raw.githubusercontent.com/pkoperwas/custom-centos9/main/ks.cfg -O /root/ks.cfg
wget http://mirroronet.pl/pub/mirrors/centos-stream/9-stream/BaseOS/x86_64/iso/CentOS-Stream-9-latest-x86_64-dvd1.iso -O /root/CentOS-Stream-9-latest-x86_64-dvd1.iso
```

**Prepare custom ISO on your linux**
```
mkdir /mnt/iso
mount -o loop /root/CentOS-Stream-9-latest-x86_64-dvd1.iso /mnt/iso
shopt -s dotglob
mkdir /root/iso
cp -pvRf /mnt/iso/* /root/iso
umount /mnt/iso
```

**Make a Note of the ISO’s Label**  currently for CentOS-Stream-9-latest-x86_64-dvd1.iso is LABEL="CentOS-Stream-9-BaseOS-x86_64"
```
blkid /root/CentOS-Stream-9-latest-x86_64-dvd1.iso
```

**Copy Kickstart File Into ISO Directory**
```
cp /root/ks.cfg /root/iso
```

**Check isolinux Menu for Linux Install Entry**
```
grep -A4 "^label linux" /root/iso/isolinux/isolinux.cfg
```

**Modify the isolinux.cfg file to Point to the New Kickstart File**
```
sed -i '/^\s*append initrd=/ s/$/ inst.ks=cdrom:\/ks.cfg/' /root/iso/isolinux/isolinux.cfg
```

**Generate the New ISO**
```
dnf install genisoimage -y
mkisofs \
-o /root/custom-centos9.iso \
-b isolinux/isolinux.bin \
-J -joliet-long -R -l -v \
-c isolinux/boot.cat \
-no-emul-boot \
-boot-load-size 4 \
-boot-info-table \
-eltorito-alt-boot \
-e images/efiboot.img \
-no-emul-boot \
-graft-points \
-V "OL-8–7–0-BaseOS-x86_64" \
-jcharset utf-8 /root/iso
```

**Make the ISO UEFI Bootable**
```
dnf install syslinux -y
isohybrid --uefi /root/custom-centos9.iso
```

**Implant Checksum for New ISO**
```
dnf install isomd5sum -y
implantisomd5 /root/custom-centos9.iso
```
