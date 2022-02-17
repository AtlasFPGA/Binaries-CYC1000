# CYC1000 Binaries 

Find the .rbf, .sof and .svf bitstream binaries under their category (arcades, computer, consoles).

Each binary includes in the filename the date when was made. 

Older versions go into old folder.



## Flash bitstream to FGPA with STM32  (rbf & AT1)

Follow specific instructions (TO BE DONE)

## Flash bitstream to FGPA with Raspberry Pi (rbf & AT1)

#### Instalación del cargador de cores programRBF en la FPGA 

Nota: vamos a trabajar desde el directorio /home/pi que es el que aparece por defecto al arrancar el terminal Linux con el usuario "pi".

* Instala la librería WiringPi:

  ```
  wget https://project-downloads.drogon.net/wiringpi-latest.deb
  sudo dpkg -i wiringpi-latest.deb
  ```

* Compila el ejecutable para cargar los cores:

  ```
  wget https://raw.githubusercontent.com/shaeon/programrbf/main/programrbf_v01.cpp
  g++ programrbf_v01.cpp -lwiringPi -o programrbf
  ```

#### Arranca los cores desde el terminal Linux de la SBC

* Copia los cores con extensión .rbf a la tarjeta SD, en la partición "rootfs", directorio ""/home/pi".

* Carga el core con el ejecutable programrbf (el ejemplo carga el [msx_atlas.rbf](./cores/msx_atlas.rbf) de MSX1):

  ```
  ./programrbf msx_atlas.rbf 17 22 4 27
  ```


#### Script para cargar un core automáticamente al arrancar la SBC

* Ejecuta los siguientes comandos en el terminal de Línux:

  ```sh
  crontab -e
  #selecciona por ejemplo nano y al final del fichero cron añade lo siguiente:
  @reboot ~/programrbf msx_multicore2.rbf 17 22 4 27
  ```

## Flash bitstream to FGPA with Desktop PC

### Quartus (sof files)  (Linux/Max/Windows)

* From IDE  (Linux / Windows)

Use the programmer tool

* From Command line (Linux / similar for windows)

Update with your own PATHs to Quartus and sof filename:

```sh
cd path/to/output_files/
export PATH="/path/to/quartus/bin:$PATH"
quartus_pgm --mode=jtag -o "p;patth/to/core.sof"

```

Example:

```sh
cd cyc1000/output_files/
export PATH="/home/jordi/bin/intelFPGA_lite/17.1/quartus/bin:$PATH"
quartus_pgm --mode=jtag -o "p;gameboy_deca.sof"

```



### OpenFPGAloader (rbf, AT1 & svf) (Linux / Mac)

* For AT1 files rename it first to rbf extension

* Install [OpenFPGAloader](https://trabucayre.github.io/openFPGALoader/guide/install.html) 

* Execute

  ```sh
  #either
  openFPGALoader -b cyc1000 filename.svf
  #or
  openFPGALoader -b cyc1000 filename.rbf
  
  ```

  

### Midnight Commander Launcher

[GNU Midnight Commander](https://midnight-commander.org/) is a free file manager, and it started as a clone of Norton Commander.

Install Midnight Commander and edit file extension scripts.  Menú command > Edit extension File.

Add following extensions to the end of the mc.ext file and adapt the script to your own environtment (mine is Linux): 

```sh
### MULTICORE ###
# sof
regex/\.sof$
	Open=/path/to/flashsof.sh %f
# svf
regex/\.svf$
	Open=/path/to/flashcyc1000_svf_rbf_openFPGAloader.sh %f
# rbf
regex/\.rbf$
	Open=/path/to/flashcyc1000_svf_rbf_openFPGAloader.sh %f

```

For reference this is the content of my own scripts:

**flashsof.sh**

```sh
export PATH="/path/to/intelFPGA_lite/17.1/quartus/bin:$PATH"
quartus_pgm --mode=jtag -o "p;$1"
```

**flashcyc1000_svf_rbf_openFPGAloader.sh**

```sh
#tested with .svf and .rbf
openFPGALoader -b cyc1000 $1
# flash SPI
#openFPGALoader -b cyc1000 -f $1
```

