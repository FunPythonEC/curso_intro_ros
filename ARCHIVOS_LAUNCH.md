# Cómo usar archivos `.launch`

Una documentación amplia sobre este formato de archivo se puede encontrar en [roslaunch/XML](http://wiki.ros.org/roslaunch/XML).

Este formato de archivo es muy parecido a XML, de hecho sigue la misma estructura. Sirve principalmente para reunir un conjunto de nodos, variables, parámetros y condiciones para que sean ejecutados en un solo comando de terminal.

Para definir un archivo para correr todos nuestros nodos a la vez seguir los siguientes pasos:

1. Crear una carpeta llamada `launch` dentro de nuestro paquete `mi_primer_nodo`.
2. Dentro de la carpeta `launch`, crear un archivo `mi_node_launcher.launch`.
3. Agregar el siguiente código dentro del archivo launch:

   ```xml
   <launch>
       <node name="talker" pkg="mi_primer_nodo" type="mi_publicador.py" output="screen"/>

       <!-- cambiar los parametros de abajo, todo lo que se encuentran en mayusculas y agregar un nodo propio -->
       <node name="NOMBRE_NODO" pkg="NOMBRE_PAQUETE" type="NOMBRE_CODIGO" output="screen"/>

   </launch>
   ```

4. Para ejecutar el archivo `.launch` se necesita abrir una terminal y correr el siguiente comando:

   ```shell
   roslaunch mi_primer_nodo mi_node_launcher.launch
   ```

   **Nota: cerrar todas las terminales que estén corriendo otros nodos, incluyendo la de roscore. Cuando se uso `roslaunch`, no es necesario tener roscore ejecutado, `roslaunch` activa el server master de ROS automáticamente.**

Ahora, vamos a ver como se define de manera propia un mensaje especifico en ROS en [como crear un mensaje especifico en ROS](CUSTOM_MSG.md).
