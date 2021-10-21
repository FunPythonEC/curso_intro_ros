# Introducción a RVIZ y Gazebo

Ambas son interfaces que nos sirven tanto para simulaciones e implementaciones reales.

## RViz

Este es básicamente el visualizador de datos de ROS. Nos permite visualizar datos en general de una manera muy gráfica, un ejemplo es el entorno del robot. Para más información, ver [RViz Docs](http://wiki.ros.org/rviz).

## Gazebo

Por otro lado Gazebo es un programa de simulación en el que se puede definir constantes físicas y trabajar con simulaciones muy cercanas a la realidad. Para más información, visitar [Gazebo Tutorials](http://gazebosim.org/tutorials?tut=ros_overview).

## A simular y experimentar!

Antes, necesitamos instalar unos paquetes con los siguientes comandos en una terminal:

```bash
sudo apt install ros-melodic-turtblebot3-gazebo ros-melodic-turtblebot3-navigation ros-melodic-dwa-local-planner
```

Ejecutar los siguientes comandos en distintas terminales:

```bash
export TURTLEBOT3_MODEL="burger"
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

y

```bash
export TURTLEBOT3_MODEL="burger"
roslaunch turtlebot3_navigation turtlebot3_navigation.launch
```

Se correrá un simulación del turtlebot3 en las cuales podrán controlar y modificar algunas partes de la interfaz. Jueguen!

Este es el final de las partes fundamentales en ROS, para otros temas avanzados, pueden seguir los [ROS tutorials](http://wiki.ros.org/ROS/Tutorials).

Finalmente para la sesión de **ROS aplicado** veremos que es un `URDF` y como pasar de un modelo 3D a una simulación en ROS como la del Turtlebot3.
