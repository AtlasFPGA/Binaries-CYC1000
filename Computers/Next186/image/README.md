# Next186SoC Atlas port by DistWave

- Next186SoC_atlas.sof  para cargar el core por JTAG o convertirlo a rbf 
- Next186SoC_atlas.jic para grabar el core en la SPI 
- El core necesita una SDHC (4GB o mayor)
- La imagen Freedos.img incluye arranque DOS con los drivers y la BIOS necesarios


sudo dd if=Freedos.img of=/dev/sdb

Néstor, [02.05.21 13:58]
La imagen en realidad sólo tiene una partición de 512 MB, pero el core necesita una SD de al menos 4 GB porque tiene que ser SDHC y las más pequeñas no lo son.
La BIOS puede ir en los primeros 64 sectores o en los últimos 16 de la SD, en la imagen freedos.img ya va incluida
La he incluido a parte por si alguien quiere montar la SD de cero con otro DOS distinto
