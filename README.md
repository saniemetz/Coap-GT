# Instalación de SO contiki 

Agregar repositorio, Instalar compilador gcc para arm
--------------------------------------------------------------------------------
			sudo apt-add-repository ppa:terry.guo/gcc-arm-embedded
			sudo apt-get update
			sudo apt-get install gcc-arm-none-eabi

Instalar desde git la versión de contiki con soporte de OpenMote 
--------------------------------------------------------------------------------
	Nota: reemplazar “user” por el usuario específico.

		sudo apt install git
		sudo git clone https://github.com/contiki-os/contiki.git /home/user/contiki-openmote 
		sudo cd contiki-openmote
		sudo git submodule update --init

# Ejemplo de procedimiento para tostar motes (OpenMote cc2538)
	Nota1: En este caso se procederá a quemar en el mote el software "border-rouer" el proceso es similar para otros programas.
	Nota2: reemplazar “user” por el usuario específico.
Instalar librería Serial de python para establecer comunicación con el mote
--------------------------------------------------------------------------------
		sudo apt-get install python-serial
Cambiarse de directorio (en este caso rpl-border-router) para generar el binario de los motes
--------------------------------------------------------------------------------		
		cd contiki-openmote/examples/ipv6/rpl-border-router
Indicar la plataforma para la que compilaremos el programa
--------------------------------------------------------------------------------
		echo 'TARGET = openmote-cc2538' > Makefile.target
		make
Conectar el mote que quiera cargarle el firmware de border-router. A continuación, para transferir el binario al microcontrolador al mote ejecutar:
--------------------------------------------------------------------------------
		make border-router.upload

	Nota: para cargar el programa en Openmote cc2538 revA1, utilizamos la siguiente función: 
		make TARGET=openmote-cc2538 BOARD_REVISION=REV_A1 PORT=/dev/ttyUSB0 border-router.upload
	Reemplazando “ttyUSB0” por el puerto en el que está conectado el mote. Para averiguarlo ejecutar:
		dmesg		
# Instructivo para utilizar la aplicación de Contiki OS "Tunslip6"
Navegamos al directorio donde se encuentra la aplicación:
--------------------------------------------------------------------------------
		cd contiki-openmote/tools
		make tunslip6
Conectamos el mote que contiene el programa border-router a la PC. Para determinar en cuál puerto serie se encuentra, en la terminal tecleamos:
--------------------------------------------------------------------------------
		dmesg

Si, por ejemplo, se conectó en el puerto serie /dev/ttyUSB0, colocamos la siguiente línea para configurar nuestro túnel.
--------------------------------------------------------------------------------
		sudo ./tunslip6 aaaa::1/64 –s /dev/ttyUSB0  (unicamente para LAN)
Si deseamos que tenga salida a internet (FRM), tecleamos:
--------------------------------------------------------------------------------
		sudo ./tunslip6 2801:1e:4007:c0da::1/64 –s /dev/ttyUSB0
	Nota: debemos tener la interfaz que conecta a internet con la siguiente IP estática: 2801:1e:4007:23::c0da:1/64

# Coap-GT

Coap para OpenMote
--------------------------------------------------------------------------------
Esta aplicacion trabaja sobre OpenMote. El codigo es una moificación de los
ejemplos desarrollados en el libro "IoT in five days" de Antonio Liñán Colina,
Alvaro Vives, Marco Zennaro, Antoine Bagula y Ermanno Pietrosemoli. En el libro
los ejemplos son desarrollados para una plataforma zolertia. Por lo que se hizo 
una pequeña adaptación.
--------------------------------------------------------------------------------
Mote con software coap
--------------------------------------------------------------------------------
Lo primero que debemos realizar es ir a la carpeta coap-gt. Alli deberemos 
cargar el software a nuestro mote. Para ello escribimos lo siguiente en la 
terminal de comandos

			make
			sudo make er-example-server.upload

--------------------------------------------------------------------------------
Mote con software rpl-border-router
--------------------------------------------------------------------------------
Para poder realizar la vision de los datos sobre Coap debemos previamente
instalar un mote con el software "border-router.c" que se encuentra en la 
siguiente ruta y accedemos. 

			cd contiki/examples/ipv6/rpl-border-router
			make

Conectamos el mote y cargamos el programa.

			sudo make border-router.upload

Este mote debera ser conectado luego mediante una interfaz serie a una PC.

--------------------------------------------------------------------------------
Tunslip6
--------------------------------------------------------------------------------
Sobre la PC debera correr una utilidad presente en las herramientas de Contiki. 
Dicha herramienta es "tunslip6". En la siguiente ruta se encuentra dicho 
programa.
			contiki/tools/

Luego deberemos ingresar el siguiente comando:

			make tunslip6

A continuación conectamos el mote con el software rpl-border-router en la PC. 
Deberemos observar en que puerto se conecta nuestro mote.
Y escribimos el siguiente comando.

			sudo ./tunslip6 -s /dev/ttyUSB0 -B 115200 fd00::1/64

--------------------------------------------------------------------------------
Navegador y visualizacion de los datos
--------------------------------------------------------------------------------
Instalar firefox la extensión "Copper".

			https://addons.mozilla.org/es/firefox/addon/copper-270430/
			
Sobre la PC deberemos abrir un navegador y colocar la direccion IPv6 del mote de
la siguiente forma.

			coap://[fd00::212:4b00:613:f56]:5683/

Entre corchetes deberemos colocar la direccion IPv6 del mote.
