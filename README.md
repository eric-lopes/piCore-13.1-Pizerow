# piCore-13.1-Pizerow
Configurando o piCore 13.1 na Placa Raspberry Pi 0 w

O Objetivo é criar um sistema retro que possa rodas jogos no DosBox, Vice (emulador de Commodore), Master System e Nintendinho, além de criar um sistema atual, porém enxuto, que possa acessar a internet, ouvir músicas e pequenos scripts python (talvez fazer análises de rede, servidor de arquivos ftp).


## Lista de TCZs Necessários #

Para o Wifi:

firmware-rpi-bt
firmware-rpi-wifi
libnl
ncurses
readline
regdb
wifi
wireless-5.10.77-piCore
wireless_tools
wpa_supplicant

Para o vim:
acl
libacl
libattr
vim

## Após criar o SD #

Remover Kernel v7 e v7l

Na mesma partição, copiar a pasta tcz

Salvar o antigo config.txt para config.txt.bkp e copiar o novo config.txt

## Primeiro Boot #

Aguardar gerar as chaves e salve-as permanentemente com:

filetool.sh -b

Depois, redimensionar a segunda partição.

sudo fdisk -u /dev/mmcblk0

Listar as partições (p), anotar o primeiro setor da segunda partição, deletá-la (d) (2) e criar uma nova partição (n) primária (p) de número (2) com início no primeiro setor da antiga. Escreva as alterações (w).

Reinicie com

sudo reboot

## Segundo Boot

Ao reiniciar, redimensionar a nova partição.

sudo resize2fs /dev/mmcblk0p2

Preparar vim, firmware-rpi e wifi para a instalação.

sudo mount /dev/mmcblkop1 /mnt/mmcblkop1
cp /mnt/mmcblkop1/tcz/* /mnt/mmcblkop2/tce/optional/
sudo rm -rf /mnt/mmcblkop1/tcz/

Instalar vim e firmware-rpi-wifi

cd /mnt/mmcblkop2/tce/optional/
tce-load -i vim.tcz
tce-load -i firmware-rpi-wifi.tcz
echo "firmware-rpi-wifi.tcz" >> ../onboot.lst

Use o vim para colocar o firmware-rpi-wifi.tcz na primeira posição no arquivo /mnt/mmcblkop2/tce/onboot.lst
Adicione vim.tcz no final deste arquivo.

Reinicie

## Terceiro Boot

Após reiniciar, instale o wifi

cd /mnt/mmcblkop2/tce/optional/
tce-load -i wifi.tcz

Use o vim para adicionar wifi.tcz abaixo do openssh no arquivo /mnt/mmcblkop2/tce/onboot.lst.

Utilize sudo wifi.sh para conectar no wifi

Adicione a linha sudo wifi.sh -a no arquivo /opt/bootlocal.sh

Salve as alterações com

filetool.sh -b

A partir de agora, o sistema estará funcionando com wifi e pronto para ser acessado via SSH (Usuário tc, Passwd piCore).

Instale alsa, mpg123, links, screen, TC (GUI) e Dillo (browser) para um uso mais fácil.

tce-load -wi alsa alsa-utils mpg123 links screen TC dillo kmaps getlocale

Comente TC.tcz no arquivo /mnt/mmcblkop2/tce/onboot.lst para iniciar em modo texto.

Adicione nozswap no início da linha (apague outros zswap) , tz=GMT+3, lang=pt_BR e kmap=qwerty/br-abnt2 (dependendo do seu teclado) no fim do arquivo cmdline.txt presente em /dev/mmcblk0p1

Definições de WiFi estão em /home/tc/wifi.db. Se alterar, utilize filetool.sh -b para salvar alterações.

Criar via GParted uma partição de SWAP de 1Gb reduzindo a segunda partição. fstab deve atualizar automaticamente.

Usar o /mnt/mmcblk0p2/tce para guardar arquivos (como músicas)

Estudar como trazer o dosbox e o vice para o piCore

