# Laboratorio 2 de Robótica
## Universiad Nacional de Colombia
## 2022-2
***
### Autores
- Andrés Holguín Restrepo 
- Julián Andrés Caipa Prieto
### Profesores encargados
- Ing. Ricardo Emiro Ramírez H.
- Ing. Jhoan Sebastian Rodriguez R.
***


## Introducción a ROS



### Introducción a Linux

Lo primero que se realizó para este laboratorio fue la instalación de Linux, en este caso, el distro Ubuntu 20.04, ya que se conocía de antemano que esta versión funcionaba adecuadamente con ROS. Hubo ciertos problemas en la instalación de Linux, sin embargo, estos estaban asociados a la creación de un OS en un disco diferente al que se tenía instalado Windows. 

De todos modos, se logró instalar el distro y funcionó adecuadamente para el resto del laboratorio. En este punto, se realizaron las pruebas iniciales de los comandos básicos del OS, verificando que todos estuvieran funcionando adecuadamente.



### Implementación Turtlesim

Ya con Linux instalado adecuadamente, fue posible implementar el Turtlesim mediante la implementación de dos consolas:

En primera terminal se ejecuta el comando *roscore* para iniciar (blah blah blah)


![Terminal roscore](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/roscore.png?raw=true)


En la segunda terminal se ejecuta el comando *rosrun turtlesim turtlesim_node* para crear la interfaz con la tortuga ubicada en el centro de esta.

![Terminal rosrun turtlesim turtlesim_node]()


### Conexión con Matlab

Siguiendo la guía, en Matlab se escribe el script inicial para trasladar linealmente en X la tortuga una unidad.

![Matlab script inicial](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/cod%20ini%20matlab.png?raw=true)

Ahora bien, al instalar Matlab no se incluyó el Toolbox de Ros, motivo por el cual era necesario añadirlo para ejecutar este script. Hubo unos problemas debido a los permisos de acceso en Linux, donde no dejaba instalar ningún add-on. Para esto, buscando en internet, se encontró la forma de otorgar estos permisos y completar la instalación, a continuación, se evidencia este código:


![Código edición permisos Matlab](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/codigo%20permisos%20matlab.png?raw=true)

Simplemente fue necesario editar el código a la versión 2020b, es decir con el comando:

sudo chown -R $LOGNAME: /usr/local/MATLAB/R2020b

Con esto funcionó correctamente la instalación del Toolbox de ROS.


### Movimiento inicial de la tortuga


Después de esto, fue fácil generar los movimientos de la tortuga. Realizando diferentes variaciones del las componentes de velMsg.Linear.X y velMsg.Linear.Y del script, se generaron diferentes movimientos como se ve en la siguiente imagen:
 ![Movimientos básicos turtlesim](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/turtlesim%20mov%20ini.png?raw=true)
 
 


### Datos *pose*



Siguiento con los ejercicios de la guía, era necesario adquirir los datos de la *pose* de la tortuga. Para esto se implementaron las siguientes líneas de código:

(blah blah blah)




Con esto se puede conocer la posición X y Y de la tortuga, y además el ángulo que tiene respecto a la horizontal, donde, teniendo en cuenta que la tortuga se encuentyra en un solo plano, es suficiente información para describir la *pose* de la tortuga.



### Edición de *pose*


Ahora bien, a pesar de que adquirir los datos de la *pose* es relativamente sencillo, ya que la *pose* es un Topic que se puede subscribir, para editar estos datos se necesitan utilizar los servicios disponibles para el *turtlesim*, después de ver la documentación, se encuentran los servicios turtleX/teleport_absolute y turtleX/teleport_relative, donde se pueden realizar cambios respecto a la pose de manera absoluta al marco de referencia global, o relativa, cambiando valores con relación a la propia tortuga.






## Programa Turtlesim


- Se debe mover hacia adelante y hacia atr´as con las teclas W y S
- Debe girar en sentido horario y antihorario con las teclas D y A.
- Debe retornar a su posici´on y orientaci´on centrales con la tecla R
- Debe dar un giro de 180° con la tecla ESPACIO



En este caso, se realizó el programa en un livescript de Matlab.




## Conclusiones

