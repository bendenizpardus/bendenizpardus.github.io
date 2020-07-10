# bendenizpardus.github.io


Merhaba arkadaşlar bu sayfada blog yazacağım, belki kaynak kodu oluşturacağım ve bilgilerimi paylaşacağım :)


## Chroot ile sisteminizi kurtarmak/tamir etmek.

kurtarmaya çalışacağınız sistem canlı usb çubuğunuzda aynı sistem olmalıdır. EFI Sistemleri icin bir kılavuz oluşturacağım. Normal BIOS için de birtane.


### chroot EFI kılavuzu
```
sudo su
lsblk 
```
Varsayılan Pardus UEFI bölümlenmesi
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

```
apt-get --reinstall install grub-common grub-efi-amd64 os-prober
update-grub
grub-install
```

### chroot BIOS/Legacy BIOS CMS kılavuzu

```
sudo su
lsblk
```
Varsayılan Pardus BIOS bölümlenmesi

```
sda      8:0    0   60G  0 disk 
├─sda1   8:1    0 10,7G  0 part /
├─sda2   8:2    0    1K  0 part 
└─sda5   8:5    0 49,3G  0 part [SWAP]
sr0     11:0    1 1024M  0 rom  
```



```
mount /dev/sda2 /mnt


for i in dev dev/pts proc sys sys/firmware; do mount --bind /$i /mnt/$i;
done

mount -o bind /etc/resolv.conf /mnt/etc/resolv.conf


chroot /mnt
```
şimdi örneğin /etc/default/grub da yaptığımız yanlış ayarları değiştirebiliriz.
Yada grubu sildiysek yeniden yükleyebiliriz:

```
apt-get --reinstall install grub-common grub-efi-amd64 os-prober
update-grub

```


```
grub-install /dev/sdX
```
`SdX` ise sizin grubun yüklü olduğu yer dir.

Yada sildiğiniz Gnome Masaüstünü yeniden yükleyebilirsiniz
```
sudo apt-get install pardus-gnome-desktop
```
