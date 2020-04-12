# Imagem de instalação

https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/

# Instruções para instalador

* Iniciar instalação expert.
* Usar cabo de rede ethernet.
* Particionar (com LVM):
  - `/var`: 10GB
  - `/tmp`: 2GB
  - `/`: 30GB
  - swap: 8GB
  - `/boot`: 300MB
  - `/home`: resto
* Escrever partição em disco usando criptografia.
* Instalar sem desktop environment.
* Instalar grub na ESP.

# Instruções pós instalação

* Reconfigurar `/etc/apt/sources.list` para usar `testing` em vez de `bullseye`, com exceção da parte de backposts.
* Instalar `gnome-core` com opção `--no-install-recommends`.
