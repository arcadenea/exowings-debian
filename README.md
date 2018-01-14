# exowings-debian
Repositorio de archivos varios e instrucciones para hacer funcionar Debian GNU/Linux en la notebook Exo Wings.


EXO 2 en 1 - Wings

Hardware:
CPU: Intel(R) Atom(TM) x5-Z8300  CPU @ 1.44GHz (CherryTrail)
RAM: 2 GB DIMM DDR3 Synchronous 1066 MHz (0,9 ns)
Video: VGA compatible controller: Intel Corporation Atom/Celeron/Pentium Processor x5-E8000/J3xxx/N3xxx Series PCI Configuration Registers (rev 22)
Audio: Intel SST (modelo exacto desconocido)
WiFi: AMPAK AP6212 - Broadcom 43438a0
Bluetooth: AMPAK AP6212 - ?
Touchscreen: ?
Touchpad: 1017:1006 Speedy Industrial Supplies, Pte., Ltd
Teclado: 1017:1006 Speedy Industrial Supplies, Pte., Ltd
Pantalla: Resolución máxima 800x1280
Acelerometro: ?
Cámara delantera: ?
Cámara trasera: ?


Instalación 
Debian 9: La instalación de Debian 9 no debería ser problemática, bootea correctamente el Live USB y el instalador nos deja un entorno funcionando, al menos con funcionalidades mínimas (sin audio, touchscreen, wifi, cámaras, control de brillo, acelerómetro y estado de la batería).
Para poder bootear el USB, ingresar a Windows y reiniciarlo manteniendo apretada la tecla SHIFT. De esta forma ingresará a un menú donde podremos seleccionar reiniciar a la configuración UEFI.

Para que no tenga problemas al arrancar, podemos configurar en la BIOS para deshabilitar los Cstates y Pstates del procesador, ya que en mi caso el kernel 4.9 tenía algunos problemas. Ni bien actualicé el sistema a un kernel mas nuevo (4.14), el sistema se estabilizó y pude activar nuevamente las opciones mencionadas.


Para tener mayor estabilidad en el sistema, es recomendable instalar un kernel nuevo desde Backports:

https://backports.debian.org/Instructions/

Luego instalar el paquete correspondiente para actualizar el kernel con el siguiente comando:

apt-get -t stretch-backports install linux-image-amd64


WiFi
La notebook integra un adaptador SDIO combinado de Wifi+Bluetooth, un AMPAK AP6212. Debian viene con el módulo preinstalado en el kernel, pero el firmware que integra no es el correcto. Es necesario bajar el firmware desde la siguiente URL:

https://github.com/arcadenea/exowings-debian/ap6212

ó alternativamente:

https://github.com/armbian/firmware/tree/master/ap6212

Luego copiar el firmware al directorio correspondiente:

cp fw_bcm43438a0.bin /lib/firmware/brcm/brcmfmac43430a0-sdio.bin
cp bcm43438a0.hcd /lib/firmware/brcm/brcmfmac43430a0-sdio.hcd
cp nvram.txt /lib/firmware/brcm/brcmfmac43430a0-sdio.txt

NOTA: hay dos versiones de firmware, esta notebook utiliza la versión "a0".

Prestar atención a los nombres de los archivos, dependiendo de la versión del kernel pide un nombre distinto. Una forma de saber el nombre correcto es ejecutando el comando "dmesg | grep brcm" y viendo la salida:

[10.674412] brcmfmac: brcmf_fw_map_chip_to_name: using brcm/brcmfmac43430a0-sdio.bin for chip 0x00a9a6(43430) rev 0x000000
[10.674536] usbcore: registered new interface driver brcmfmac
[10.681258] brcmfmac mmc1:0001:1: firmware: direct-loading firmware brcm/brcmfmac43430a0-sdio.bin
[10.693967] brcmfmac mmc1:0001:1: firmware: direct-loading firmware brcm/brcmfmac43430a0-sdio.txt
[10.786513] brcmfmac: brcmf_c_preinit_dcmds: Firmware version = wl0: Jun  6 2014 14:50:39 version 7.10.226.49 (r) FWID 01-8962686a

En este caso verán que pide los archivos "brcmfmac43430a0-sdio.bin" y "brcmfmac43430a0-sdio.txt"

Teclado:
Funciona correctamente.


Touchpad:
Funciona correctamente.

Bluetooth
Todavía no pude hacerlo funcionar.


Touchscreen
Todavía no pude hacerlo funcionar.


Audio
Todavía no pude hacerlo funcionar.


Pantalla
La pantalla viene por defecto configurada en modo vertical, para rotarla, podemos usar el siguiente comando: xrandr -o left

