# Cómo crear un mensaje custom en ROS

Para un proceso mucho más detallado pueden seguir el tutorial de [Crear un mensaje y servicio](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv). En el enlace anterior podran encontrar cómo se genera un mensaje propio y además el de un servicio en ROS.

En este taller, no se llegará a generar servicios sin embargo pueden verlo en [Escribiendo un servicio y un cliente](http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29).

## Definir archivo `.msg` y compilarlo

Para un mensaje, se deben de seguir los siguientes pasos:

1. Crear un directorio o carpeta llamado `msg` dentro de el paquete `mi_primer_nodo`.
2. Dentro del directorio `msg`, crear un archivo `VelocidadLlantas.msg`.
3. Dentro del archivo de mensaje agregar lo siguiente:

   ```yaml
   int64 velocidad_izquierda
   int64 velocidad_derecha
   ```

4. Dentro del archivo `package.xml`, descomentar las siguientes lineas:

   ```xml
   <build_depend>message_generation</build_depend>
   <exec_depend>message_runtime</exec_depend>
   ```

5. Editar el archivo `CMakeLists.txt` y verificar las siguientes líneas:

   ```xml
   find_package(catkin REQUIRED COMPONENTS
      roscpp
      rospy
      std_msgs
      message_generation
   )
   ```

6. Verificar las siguientes líneas:

   ```xml
    catkin_package(
     ...
     CATKIN_DEPENDS message_runtime ...
     ...)
   ```

7. Verificar las siguientes líneas:

   ```xml
   add_message_files(
     FILES
     VelocidadLlantas.msg
   )
   ```

8. Verificar las siguientes líneas:

   ```xml
   generate_messages(
     DEPENDENCIES
     std_msgs
   )
   ```

9. Ejecutar `catkin_make` en el directorio del espacio de trabajo principal `curso_ros`.

## Comprobar nuestro `msg`

Para comprobar que se haya compilado de manera correcta, puede checkear con el siguiente comando:

```bash
rosmsg list | grep Velocidad
```

Deberían obtener algo parecido a lo siguiente:

![rosmsg_list](/media/rosmsg_list.png)

Ahora a usar el mensaje, como reto, modificar uno de sus nodos publicadores para que lo use y envíe los números a un topic, tener en cuenta que para asignar los valores se debe de realizar de la siguiente manera:

Para importar el mensaje desde Python:

```python
from mi_primer_nodo.mg import VelocidadLlantas
```

Y para asignar valores al mensaje:

```python
vel_msg = VelocidadLlantas()
vel_msg.velocidad_izquierda=1
vel_msg.velocidad_derecha=1
```

Publicando datos a un topic, podemos hacer uso de un graficador en tiempo real de ROS llamado `rqt_plot`. Un tutorial sobre su uso se puede encontrar en [Debugging con rqt_plot](https://roboticsbackend.com/rqt-plot-easily-debug-ros-topics/).

## Un pequeño reto

Desde uno de los nodos, publicar con el mensaje de VelocidadLlantas valores continuos periodicos y graficarlos con rqt_plot.

Una vez llegada a esta etapa, revisaremos finalmente una de las herramientas más importantes de ROS, estas son RViz y Gazebo, para ello seguir la siguiente guía [introducción a RViz y Gazebo](RVIZ_GAZEBO_INTRO.md).
