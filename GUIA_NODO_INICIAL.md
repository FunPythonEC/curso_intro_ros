# Guía para generar un nodo subscriptor y publicador

Para generar un nodo y comenzar un el sistema de software del robot, es necesario establecer un espacio de trabajo de ROS.

## Cómo crear un espacio de trabajo con ROS

Para ello seguir la siguiente guía: [create catkin workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace)

O ejecutar los siguientes comandos:

```bash
mkdir -p ~/curso_ros/src
cd ~/curso_ros/
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
```

Para poder acceder a todo lo respecto a los paquetes del espacio de trabajo, ejecutar `source devel/setup.bash` dentro del espacio de trabajo cada vez que se abra una nueva terminal.

Si no se desea tener que correr el ultimo comando cada vez que se abra una terminal, correr lo siguiente:

```bash
echo "source ~/curso_ros/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## Cómo generar un paquete en un espacio de trabajo

Seguir la guía de [crear un paquete](http://wiki.ros.org/catkin/Tutorials/CreatingPackage) o ejecutar los siguientes comandos:

```bash
cd ~/curso_ros/src
catkin_create_pkg mi_primer_nodo std_msgs rospy roscpp
cd ~/curso_ros
catkin_make
```

## Generando un publicador

Casi todas de las acciones de ROS se pueden realizar tanto por terminal como por código. Nosotros usaremos Python para todos los ejemplos.

Seguir los siguientes pasos para crear un publicador con Python:

1. Crear una carpeta llamada `scripts` dentro del paquete o la carpeta `mi_primer_nodo`.
2. Crear un archivo llamado `mi_publicador.py` dentro de la carpeta `scripts`.
3. Escribir el siguiente código en el archivo:

**Para la creación de archivos se puede usar `touch` y `nano`. Dentro de una terminal que se encuentre en el directorio correspondiente, ejecutar `nano mi_publicador.py` para crear el archivo y editarlo. Para guardarlo usar `CTRL+O + Enter` y para cerrar `CTRL+X`.**

```python
#!/usr/bin/env python3

import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10)
    while not rospy.is_shutdown():
        hello_str = "hola mundo %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```

**Nota: Si se reciben errores sobre permisos, correr el siguiente comando en la terminal: `chmod +x <direccion_archivo_codigo>`. Este comando nos permite dar permisos de ejecución al código.**

Antes de correr cualquier comando de ROS, se necesita tener al ROS Master ejecutando en una terminal con:

```bash
roscore
```

El comando de roscore solo debe ser ejecutado en una terminal a la vez.

Para correr el publicador se puede realizar a través de la ejecución del siguiente comando en una terminal:

```bash
rosrun mi_primer_nodo mi_publicador.py
```

**Nota: Si se reciben errores sobre permisos, correr el siguiente comando en la terminal: `chmod +x <direccion_archivo_codigo>`**

### Comprobar el funcionamiento

Para comprobar el funcionamiento, se puede ver si el nodo esta corriendo con el comando:

```bash
rqt_graph
```

O

```bash
rosrun rqt_graph rqt_graph
```

También se puede recibir los datos desde la terminal haciendo uso de `rostopic`:

```bash
rostopic echo /chatter
```

### Publicar desde la terminal

Se hace uso del siguiente comando:

```bash
rostopic pub -r 10 /chatter std_msgs/String "data: 'hola'"
```

## Generando un subscriptor

Para la generación del nodo subscriptor, crear un archivo `mi_subscriptor.py` en la carpeta `scripts`.

Añadir el siguiente código en `mi_subscriptor.py`:

```python
#!/usr/bin/env python3
import rospy
from std_msgs.msg import String

def callback(data):
    # en esta funcion se debe de realizar todo lo deseado con el mensaje o datos que se reciben.
    rospy.loginfo(rospy.get_caller_id() + "El mensaje ha sido %s", data.data)

def listener():
    rospy.init_node('listener', anonymous=True)

    rospy.Subscriber("chatter", String, callback)

    rospy.spin()

if __name__ == '__main__':
    listener()
```

Para comprobar el funcionamiento se puede realizar de la misma manera que con el publicador.

## Generar un nodo suscriptor y publicador

Usando el siguiente código:

```python
#!/usr/bin/env python3

import rospy
from std_msgs.msg import String

rospy.init_node('mix_node', anonymous=True)
pub = rospy.Publisher('mix_message', String, queue_size=10)
def callback(data):
    msg = String()
    msg.data = data.data+"mix"
    pub.publish(msg)

rospy.Subscriber("chatter", String, callback)

rate = rospy.Rate(10)
while not rospy.is_shutdown():
    rate.sleep()
```

## Y ahora que?

Con los nodos ya definidos, se vuelve tedioso tener que abrir una terminal por cada uno de estos, para eso se hace uso de archivos `.launch`.

El siguiente paso, es [Cómo usar archivos `.launch`](ARCHIVOS_LAUNCH.md).
