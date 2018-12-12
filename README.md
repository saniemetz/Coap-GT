# Coap-GT

Coap para OpenMote
--------------------------------------------------------------------------------
Esta aplicacion trabaja sobre OpenMotes. El codigo es una moificación de los
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
Sobre la PC deberemos abrir un navegador y colocar la direccion IPv6 del mote de
la siguiente forma.

			coap://[fd00::212:4b00:613:f56]:5683/

Entre corchetes deberemos colocar la direccion IPv6 del mote.
