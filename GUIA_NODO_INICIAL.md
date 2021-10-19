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

## Cómo generar un paquete en un espacios de trabajo

Seguir la guía de [crear un paquete](http://wiki.ros.org/catkin/Tutorials/CreatingPackage)

O ejecutar los siguientes comandos:

```bash
cd ~/curso_ros/src
catkin_create_pkg mi_primer_nodo std_msgs rospy roscpp
cd ~/curso_ros
catkin_make
```
