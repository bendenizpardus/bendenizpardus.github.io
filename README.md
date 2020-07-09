# bendenizpardus.github.io
Pardus

Merhaba arkadaşlar bu sayfada blog yazacağım, belki kaynak kodu oluşturacağım ve bilgilerimi paylaşacağım :)


Chroot ile sisteminizi kurtarmak.

kurtarmaya çalışacağınız sistem canlı usb çubuğunuzda aynı sistem olmalıdır. EFI Sistemleri icin bir kılavuz oluşturacağım. Normal BIOS için de birtane.


##EFI kılavuzu
```
sudo su
lsblk 
```

```
sda      8:0    0   60G  0 disk 
├─sda1   8:1    0  512M  0 part /boot/efi
├─sda2   8:2    0 11,7G  0 part /
└─sda3   8:3    0 47,9G  0 part [SWAP]
sr0     11:0    1  1,6G  0 rom  /media/cdrom0

```


```
mount /dev/sda2 /mnt
mount /dev/sda1 /mnt/boot/efi
```

```
for i in dev dev/pts proc sys sys/firmware; do mount --bind /$i /mnt/$i;
done
```

```
mount -o bind /etc/resolv.conf /mnt/etc/resolv.conf
```
şimdi sistemi değiştirmek istiyoruz



```
chroot /mnt
```
şimdi sistemi tamir edebiliriz

##BIOS kılavuzu

```
mount /dev/sda2 /mnt


for i in dev dev/pts proc sys sys/firmware; do mount --bind /$i /mnt/$i;
done
mount -o bind /etc/resolv.conf /mnt/etc/resolv.conf


chroot /mnt
```
