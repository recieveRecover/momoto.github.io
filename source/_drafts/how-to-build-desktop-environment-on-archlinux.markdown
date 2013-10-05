pacman -S xorg-server xorg-xinit xorg-server-utils
pacman -S xf86-video-ati
pacman -S virtualbox-guest-utils

pacman -S openbox
pacman -S obconf obmenu tint2 pcmanfm dmenu obkey oblogout

pacman -S slim slim-themes archlinux-themes-slim
systemctl enable slim.service

pacman -S mate gdm
systemctl enable gdm

cp jp106.map.gz personal.map.gz
gunzip personal.map.gz
vi personal.map.gz
gzip personal.map

