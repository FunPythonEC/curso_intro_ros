# Introducción a RVIZ y Gazebo

Ambas son interfaces que nos sirven tanto para simulaciones e implementaciones reales.

## RViz

Este es basicamente el visualizador de datos de ROS. Nos permite visualizar datos en general de una manera muy gráfica, un ejemplo es el entorno del robot.

## Gazebo

Por otro lado Gazebo es un programa de simulación en el que se puede definir constantes físicas y trabajar con simulaciones muy cercanas a la realidad.

## A simular y experimentar!

Ejecutar los siguientes comandos en distintas terminales:

```bash
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

y

```bash
roslaunch turtlebot3_navigation turtlebot3_navigation.launch
```

Se correrá un simulación del turtlebot3 en las cuales podrán controlar y modificar algunas partes de la interfaz. Jueguen!

Este es el final de las partes fundamentales en ROS, para otros temas avanzados, pueden seguir los [ROS tutorials](http://wiki.ros.org/ROS/Tutorials).

Finalmente para la sesión de **ROS aplicado** veremos que es un `URDF` y como pasar de un modelo 3D a una simulación en ROS como la del Turtlebot3.
