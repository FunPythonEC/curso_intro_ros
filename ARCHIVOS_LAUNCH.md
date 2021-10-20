# Cómo usar archivos `.launch`

Una documentación amplia sobre este formato de archivo se puede encontrar en [roslaunch/XML](http://wiki.ros.org/roslaunch/XML).

Este formato de archivo es muy parecido a un XML, de hecho sigue la misma estructura. Sirve principalmente para reunir un conjunto de nodos y variables para que sean ejecutados en un solo comando de terminal.

Para definir un archivo para correr todos nuestros nodos a la vez seguir los siguientes pasos:

1. Crear una carpeta dentro de nuestro paquete llamada `launch`.
2. Dentro de la carpeta crear un archivo `mi_node_launcher.launch`.
3. Agregar el siguiente código dentro del archivo launch:

```xml
<launch>
    <node name="talker" pkg="mi_primer_nodo" type="mi_publicador.py" output="screen"/>

    <node name="NOMBRE_NODO" pkg="NOMBRE_PAQUETE" type="NOMBRE_CODIGO" output="screen"/>
</launch>
```

4. Para ejecutar el archivo `.launch` se necesita abrir una terminal y correr el siguiente comando:

```shell
roslaunch mi_primer_nodo mi_node_launcher.launch
```

Ahora, vamos a ver como se define de manera propia un mensaje especifico en ROS en [como crear un mensaje especifico en ROS](CUSTOM_MSG.md).
