# bendenizpardus.github.io


Merhaba arkadaşlar bu sayfada bloğumu yazacağım, belki kaynak kodu oluştururum ve bilgilerimi paylaşırım :)


## Pardusda, yüklü bulunan sistemimizden live-ISO üretmek:

Bu yöntem yeni ve temiz yüklenmiş bir sisteme uygulayınız. Çünkü ISO nun boyutu ona göre artacak. Benim tavsiyem sistemi yükleyiniz sonra, olmazsa olmaz yazılımlarınızı yükleyiniz. Ve ondan sonra ISOyu üretiniz.

Uçbirimini açmak için XFCEde ctrl+alt+t tuşlarına basıyoruz.
Gnomede ise Masaüstüne sağ tıklayarak uçbirimini açıyoruz.

```
sudo apt update

sudo apt instal -y squashfs-tools build-essential dkms linux-headers-$(uname -r) aufs-dkms wget zip unzip

cd /tmp/

wget https://github.com/Tomas-M/linux-live/archive/master.zip

unzip master.zip

cd linux-live-master

sudo su

chmod +x build

./build        Bu adım çok uzun sürecek!

cd ..

chmod +x gen_linux_iso.sh 

./gen_linux_iso.sh

chown kulanıcıisminiz linux-x86_64.iso

```

Sonra üretilmiş olan ISOyu Rufus yada etcher:
https://www.balena.io/etcher/
https://github.com/balena-io/etcher

İle USB beleğinize yükleyebilirsiniz

Kaynaklar:

https://www.linux-live.org/
http://www.linuxandubuntu.com/home/make-your-very-own-customized-linux-distro-from-your-current-installation



## Chroot ile sisteminizi kurtarmak/tamir etmek:

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
Video açıklaması [Link](https://youtu.be/RHXIuA26-h4).

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
mount /dev/sda1 /mnt


for i in dev dev/pts proc sys sys/firmware; do mount --bind /$i /mnt/$i;
done

mount -o bind /etc/resolv.conf /mnt/etc/resolv.conf


chroot /mnt
```
şimdi örneğin `/etc/default/grub` da yaptığımız yanlış ayarları değiştirebiliriz.
Yada grubu sildiysek yeniden yükleyebiliriz:

```
apt-get --reinstall install grub-common grub-efi-amd64 os-prober
update-grub

```


```
grub-install /dev/sdX
```
`sdX` ise sizin grubun yüklü olduğu yer dir.

Yada sildiğiniz Gnome Masaüstünü yeniden yükleyebilirsiniz
```
sudo apt-get install pardus-gnome-desktop
```

Video açıklaması [Link](https://youtu.be/6JRoDVnd3N0).

## Markdown temelleri ( ingilizce):

https://www.markdownguide.org/basic-syntax/ [Link](https://www.markdownguide.org/basic-syntax/).

İçeriğimi beğendiyseniz Youtube kanalıma bir göz atabilirsinz :) [Link](https://www.youtube.com/channel/UC0AdzNi_mW1-QEJ_fuJFoWw/featured).
