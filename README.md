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

En primera terminal se ejecuta el comando *roscore*, el cual se trata del comando principal de ROS, ya que permite el funcionamiento de los nodos, y al ejecutarlo se pone en marcha el nodo central o maestro. Al ejecutar el comando, la consola muestra lo siguiente:


![Terminal roscore](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/roscore.png?raw=true)


En la segunda terminal se ejecuta el comando *rosrun turtlesim turtlesim_node* para crear la interfaz con la tortuga ubicada en el centro de esta. Al ejecutar el comando se evidencia lo dicho previamente, la creación del respectivo nodo y el posicionamiento dado:

![Terminal rosrun turtlesim turtlesim_node]()


### Conexión con Matlab

Siguiendo la guía, en Matlab se escribe el script inicial para trasladar linealmente en X la tortuga una unidad. El código es fácilmente entendible bajo el concepto de los nodos de ROS. Inicialmente se realiza una conexión con el nodo maestro, y luego se define un Publisher o publicador del nodo de turtlesim creado, lo cual es como una especie de objeto que envía información de atributos, almacenados en el mensaje. A este mensaje es posible asignarle valores a los parámetros que lo componen y enviarlo para actualizar el estado del nodo, y por ende generar el movimiento de la turtlesim. 

![Matlab script inicial](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/cod%20ini%20matlab.png?raw=true)

Ahora bien, al instalar Matlab no se incluyó el Toolbox de ROS, motivo por el cual era necesario añadirlo para ejecutar este script. Hubo unos problemas debido a los permisos de acceso en Linux, donde no dejaba instalar ningún add-on. Para esto, buscando en internet, se encontró la forma de otorgar estos permisos y completar la instalación, a continuación, se evidencia este código:


![Código edición permisos Matlab](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/codigo%20permisos%20matlab.png?raw=true)

Simplemente fue necesario editar el código a la versión 2020b, es decir con el comando:

sudo chown -R $LOGNAME: /usr/local/MATLAB/R2020b

Con esto funcionó correctamente la instalación del Toolbox de ROS.

Ahora bien, para desconectar samonda



### Movimiento inicial de la tortuga


Después de esto, fue fácil generar los movimientos de la tortuga. Realizando diferentes variaciones del las componentes de velMsg.Linear.X y velMsg.Linear.Y del script, se generaron diferentes movimientos como se ve en la siguiente imagen:

![Movimientos básicos turtlesim](https://github.com/aholguinr/Lab2_Robotica_Caipa_Holguin/blob/main/Fotos/turtlesim%20mov%20ini.png?raw=true)

Al modificar dichos parámetros es posible desplazar la tortuga de su posición original a alguna asignada por los valores determinados.

### Datos *pose*

Siguiento con los ejercicios de la guía, era necesario adquirir los datos de la *pose* de la tortuga. Para esto se implementaron las siguientes líneas de código:

```
topic='/turtle1/cmd_vel'
msgtype='geometry_msgs/Twist'

velPub=rospublisher(topic,msgtype)
velMsg=rosmessage(velPub)

sub.LatestMessage.Linear
sub.LatestMessage.Angular
```


Con esto se puede conocer la posición X y Y de la tortuga, y además el ángulo que tiene respecto a la horizontal, donde, teniendo en cuenta que la tortuga se encuentra en un solo plano, es suficiente información para describir la *pose* de la tortuga.


### Edición de *pose*

Ahora bien, a pesar de que adquirir los datos de la *pose* es relativamente sencillo, ya que la *pose* es un Topic que se puede subscribir, para editar estos datos se necesitan utilizar los servicios disponibles para el *turtlesim*, después de ver la documentación, se encuentran los servicios turtleX/teleport_absolute y turtleX/teleport_relative, donde se pueden realizar cambios respecto a la pose de manera absoluta al marco de referencia global, o relativa, cambiando valores con relación a la propia tortuga.




## Programa Turtlesim

Se debe implementar un programa que permita las siguientes funcionalidades con la tortuga de la interfaz de turtlesim:

- Se debe mover hacia adelante y hacia atrás con las teclas W y S
- Debe girar en sentido horario y antihorario con las teclas D y A.
- Debe retornar a su posición y orientación centrales con la tecla R
- Debe dar un giro de 180° con la tecla ESPACIO

Para la realización del programa, se decidió continuar utilizando Matlab debido a la facilidad de uso de este. Con lo aprendido en los puntos anteriores, para editar la pose de la tortuga, se utilizan los servicios de "/turtle1/teleport_relative" y "/turtle1/teleport_absolute". También se subscribe al tópico “/turtle1/pose" para conocer la pose actual de la tortuga.
Para identificación de teclas pulsadas, dentro de una figura vacía, se utiliza la función waitforbuttonpress y se obtiene el valor obtenido, donde se tiene que estos valores obtenidos son:
- W:119
- A: 97
- S: 115
- D: 100
- R: 114
- Espacio: 32
- E: 101

A partir de esto, se realiza un ciclo while hasta que se presione la tecla E, referente a Exit o Escape del programa y cierre funcionalidades. Hasta que eso no suceda, dependiendo de las entradas generadas por tecla presionada, se cambian los valores de pose de la tortuga en pasos lineares de 0.25 para adelante y para atrás, y pasos angulares de 0.25 radianes horarios y antihorarios, todo eso mediante el servicio “/turtle1/teleport_relative". Este servicio también se utiliza para el giro de 180 grados, donde se adquiere el valor de theta de la pose mediante el subscriber del mismo, y si este valor es menor a 0, se añaden pi radianes, de lo contrario, se restan pi radianes. Por último, para volver a la posición inicial, se tiene en cuenta que estos valores siempre corresponden en X y Y de 5.44445 debido a las especificaciones de la ventana de la tortuga, por lo que se utiliza el servicio "/turtle1/teleport_absolute" y se asignan estos valores X y Y, y un valor theta de 0, correspondiente a las condiciones iniciales.
Con todo y lo anterior, este resultado puede evidenciarse en el archivo Lab2.mlx del repositorio, y en el video “resultados.mp4” el cual se grabó desde un celular para que se identifique de mejor forma las pulsaciones de teclado y los cambios generados en la tortuga.




## Conclusiones

