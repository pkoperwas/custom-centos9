**Download files:**
```
Downoload ks.cfg from this repo to /root/ks.cfg
Download 11GB http://mirroronet.pl/pub/mirrors/centos-stream/9-stream/BaseOS/x86_64/iso/CentOS-Stream-9-latest-x86_64-dvd1.iso to /root/CentOS-Stream-9-latest-x86_64-dvd1.iso
```

**Prepare custom ISO on your linux**
```
mount -o loop /root/CentOS-Stream-9-latest-x86_64-dvd1.iso /mnt/iso
shopt -s dotglob
mkdir /root/iso
cp -pvRf /mnt/iso/* /root/iso
umount /mnt/iso
```

**Make a Note of the ISO’s Label**
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
