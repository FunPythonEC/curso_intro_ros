# Cómo pasar de OnShape a ROS

Para pasar un diseño de OnShape a ROS se usará una librería de Python3 llamada [OnShape to Robot](https://github.com/Rhoban/onshape-to-robot).

Primero debemos instalarla.

## Instalación de Onshape To Robot

Para instalar esta librería ejecutar el comando:

```bash
sudo pip3 install onshape-to-robot
```

Una vez instalado, necesitamos permitir que la librería de onshape-to-robot tenga acceso a nuestra cuenta de OnShape, para eso se debe de ingresar al siguiente link [OnShape developer portal](https://dev-portal.onshape.com/keys). En esta página seguir los siguientes pasos:

1. Dar click en el boton `Create new API key`.

   ![onshape_new_key](/media/onshape_new_key.png)

2. Dar click a todos los checkboxes (solo por si acaso den permiso a todo).

   ![onshape_permissions](/media/onshape_permissions.png)

3. Dar click en `Create API key` y guardar la API key y secret key copiandolas en algún archivo cualquiera.

## Exportar diseño de OnShape

Para finalmente exportar el diseño de OnShape, seguir los siguientes pasos:

1. Abrir una terminal y crear una carpeta llamada `mi_robot`.
2. Dentro de la carpeta `mi_robot`, crear un archivo llamado `config.json`.
3. Abrir el archivo `config.json` y pegar el siguiente texto:

   ```json
   {
     "onshape_api": "https://cad.onshape.com",
     "onshape_access_key": "[KEY]", //reemplazar por clave de API
     "onshape_secret_key": "[SECRET]", //reemplazar por clave secreta

     "documentId": "[document-id]", //reemplazar por https://cad.onshape.com/documents/XXXXXXXXX/w/YYYYYYYY/e/ZZZZZZZZ las letras XXXXXXXXXX del link de su proyecto en onshape
     "outputFormat": "urdf"
   }
   ```

4. Fuera de la carpeta `mi_robot`, correr el siguiente comando:

```bash
onshape-to-robot mi_robot
```

Dentro de la carpeta `mi_robot` aparecera unos archivos, entre este un URDF el cual contiene la geometría del robot.

## A pasarlo a Gazebo

Ahora necesitamos mover estos archivos hacia un paquete de ROS y hacer unas modificaciones, para ello seguir los siguientes pasos:

1.  Crear un nuevo paquete dentro de la carpeta `src` en el workspace de ROS ejecutando lo siguiente:

    ```bash
    catkin_create_pkg mi_robot_description std_msgs rospy roscpp
    ```

2.  Dentro de `mi_robot_description`, crear una carpeta llamada `urdf`.
3.  Entrar con la terminal a la carpeta `urdf` y correr el siguiente comando:

    ```bash
    cp ~/mi_robot/robot.urdf .
    ```

4.  En el paquete `mi_robot_description`, crear la carpeta `meshes`.

5.  Abrir una terminal en la carpeta `meshes` y correr los siguientes comandos:

    ```bash
    cp ~/mi_robot/*.part .
    cp ~/mi_robot/*.stl .
    ```

6.  Ahora modificaremos el archivo `robot.urdf` en las siguientes lineas:

    En todas las lineas donde se encuentre `package://`, agregar `mi_robot_description/meshes/` despues de los slashes, como de la siguiente manera:

    ```xml
    <geometry>
    <mesh filename="package://mi_robot_description/meshes/rueda_loca.stl"/>
    </geometry>
    ```

7.  Luego se tiene que modificar la sección de dentro del tag geometry que esta dentro de colision para las llantas para que quede de la siguiente manera:

    ```xml
    <collision>
    <origin xyz="0 0 0.02" rpy="3.14159 -0 0" />
    <geometry>
    <!-- Se cambia a un elemento cilindro -->
    <cylinder length="0.020" radius="0.025"/>
    </geometry>
    <material name="llanta_material">
    <color rgba="0 0 0 1.0"/>
    </material>
    </collision>
    ```

8.  Cambiar el tipo de junta de las llantas a continuous:

    ```xml
    <joint name="link2" type="continuous">
    ```

9.  Al inicio del archivo, despues del tag `robot`, definir un eslabon llamado `base_footprint` como se ve ahora:

    ```xml
    <robot name="onshape">
    <link name="base_footprint"/>
    ```

10. Luego de la definición del eslabón base, agregar el código de junta de la base con el eslabón base_footprint:

    ```xml
    <joint name="base_joint" type="fixed">
            <parent link="base_footprint"/>
            <child link="base"/>
            <origin xyz="0.0 0.0 0.025" rpy="0 0 0"/>
    </joint>
    ```

Para darle color en Gazebo se debe de usar el siguiente XML:

```xml
<gazebo reference="llanta_2">
    <mu1>1.6</mu1>
    <mu2>1.6</mu2>
    <kp>500000.0</kp>
    <kd>10.0</kd>
    <minDepth>0.001</minDepth>
    <maxVel>1</maxVel>
    <fdir1>1 0 0</fdir1>
    <material>Gazebo/FlatBlack</material>
</gazebo>
```

Ahora si, intentemos simular nuestro robot.

## Lanzando el robot

Para simularlo, es necesario armar un archivo `.launch`. Entonces sigamos los pasos:

1. Crear la carpeta `launch` dentro del paquete de `description`.
2. Crear archivo `robot_launcher.launch`.
3. Copiar el siguiente código dentro del archivo launch:

```xml
<launch>

    <arg name="x_pos" default="0"/>
    <arg name="y_pos" default="0"/>
    <arg name="z_pos" default="0"/>
    <arg name="roll" default="0"/>
    <arg name="pitch" default="0"/>
    <arg name="yaw" default="1.57"/>

    <arg name="urdf_file" default="$(find xacro)/xacro --inorder '$(find mi_robot_description)/urdf/rob$

    <param name="robot_description" command="$(arg urdf_file)" />
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />
    <node pkg="robot_state_publisher" type="robot_state_publisher" name="robot_state_publisher">
        <param name="publish_frequency" type="double" value="50.0" />
    </node>


        <include file="$(find gazebo_ros)/launch/empty_world.launch">
            <arg name="world_name" value="$(find turtlebot3_gazebo)/worlds/turtlebot3_world.world"/>
            <arg name="paused" value="false"/>
            <arg name="use_sim_time" value="true"/>
            <arg name="gui" value="true"/>
            <arg name="headless" value="false"/>
            <arg name="debug" value="false"/>
        </include>
        <node pkg="gazebo_ros" type="spawn_model" name="spawn_urdf" args="-urdf -model mi_robot -x $(arg x_pos) -y $(arg y_pos) -z $(arg z_pos) -R $(arg roll) -P $(arg pitch) -Y $(arg yaw) -param robot_description" />
</launch>

```

**Nota: Si Gazebo se pone necio, usar el comando `killall -9 gzserver ` después de cerrar el programa.**

Ahora hace falta agregar sensores al robot y mover el robot, para ello si es de manera simulada, debemos agregar unos plugins de Gazebo en [Agregar plugins de Gazebo a nuestro robot](./GAZEBO_PLUGINS.md)
