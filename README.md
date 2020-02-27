# my-macbook-triple-boot

# Planejamento

- Utilizar LVM para construir uma partição bruta em separado como o APFS.
- Utilizar refind para bootloader.
- Aplicar MBR protegido.
- Aplicar criptografia na partição /home (por enquanto não se preocupar com
swap file).
- Usar Ubuntu server installer?

# Pré instalação

- Criar um sistema de partições limpo usando GPT
- Garantir que MBR está em modo protegido no fim
- Criar primeira partição com tamanho 500 MiB de tipo EFI Boot Partition.

Veja [3] para os passos a seguir...

```shell
pvcreate /dev/sda?
vgcreate mondrian /dev/sda?
lvcreate --name boot --size 300M mondrian
lvcreate --name root --size 28G mondrian
lvcreate --name home --size 100G mondrian
```

Para criptografia veja [2] e [7].

# Intermediário

- `grub-install --target=x86_64-efi /dev/sda`
- Mudar MBR para modo protegido.
- Configurar mounts no `/etc/fstab`
- swapfile pode ser feito dentro da partição criptografada (veja [8])

# Pós-instalação

```shell
# Remova todas as snap packages...
apt remove --purge snapd printer-driver-brlaser printer-driver-foo2zjs \
printer-driver-foo2zjs-common printer-driver-m2300w printer-driver-ptouch \
printer-driver-splix ubuntu-advantage-tools popularity-contest whoopsie \
ubuntu-software gnome-software apport gnome-shell-extension-appindicator \
gnome-shell-extension-ubuntu-dock gsettings-ubuntu-schemas ubuntu-session
apt-get autoremove --purge
apt-get autoclean
apt-get clean

apt-get update
apt-get install --no-install-recommends curl gnome-tweaks apt-transport-https \
gnome-session
apt-get dist-upgrade --no-install-recommends
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get install --no-install-recommends sublime-text
apt-get autoremove --purge
```

```shell
echo GTK_IM_MODULE=cedilla >> /etc/environment
```

```shell
wget -O gnome-shell-extension-installer "https://github.com/brunelli/gnome-shell-extension-installer/raw/master/gnome-shell-extension-installer"
chmod +x gnome-shell-extension-installer
gnome-shell-extension-installer 19
rm gnome-shell-extension-installer
```

```shell
sudo update-alternatives --all
```

- Baixar fontes: Cantarell, a fonte Fira Code Z está no repositório
- Baixar e aplicar temas do Mondrian
- Aplicar fontes e desativar antialiasing RGB, desativar hinting.
- Fazer o git funcionar com o seahorse: https://askubuntu.com/questions/773455/what-is-the-correct-way-to-use-git-with-gnome-keyring-and-https-repos
- Aplicar configurações de DNS [9]

# Ativar Refind

- Fazer manual bootstanzas.
- Configurar modo gráfico.
- Configurar scan_for
- Construir imagem de 1 pixel black e aplicar como tema (desativar banner)

# Referências

- [1] https://wiki.archlinux.org/index.php/LVM
- [2] https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system
- [3] https://www.howtoforge.com/linux_lvm
- [4] https://bbs.archlinux.org/viewtopic.php?id=178158
- [5] https://askubuntu.com/questions/1066028/install-ubuntu-18-04-desktop-with-raid-1-and-lvm-on-machine-with-uefi-bios
- [6] https://itsfoss.com/create-swap-file-linux/
- [7] https://askubuntu.com/questions/1032546/should-i-use-luks1-or-luks2-for-partition-encryption
- [8] https://superuser.com/questions/1067150/how-to-create-swapfile-on-ssd-disk-with-btrfs/1411462#1411462
- [9] https://developers.cloudflare.com/1.1.1.1/setting-up-1.1.1.1/
