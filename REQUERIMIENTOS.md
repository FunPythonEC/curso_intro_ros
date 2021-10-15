# Requerimientos del curso

Actualmente ROS funciona y es estable primordialmente en sistemas Linux, por lo tanto para el curso, la instalación de Linux es un requisito muy importante. Instalaremos Ubuntu 18 en una maquina virtual usando VMware. ROS y todo el resto de la herramientas deberán ser instaladas en la maquina virtual de Ubuntu. Se usará solamente linux para todo el curso.

Si ya tienen linux instalado, pueden saltar a la sección de instalación de ROS más abajo.

En las siguiente secciones se especifica como instalar cada una de estas herramientas.

## Instalación de VMWare Workstation 16 y Ubuntu 18.04

A continuación se detallan los pasos a seguir:

1. Descargar el archivo .iso de Ubuntu 18 de [https://releases.ubuntu.com/18.04/ubuntu-18.04.6-desktop-amd64.iso](https://releases.ubuntu.com/18.04/ubuntu-18.04.6-desktop-amd64.iso).
2. Descargar VMWare de [https://www.vmware.com/latam/products/workstation-player/workstation-player-evaluation.html](https://www.vmware.com/latam/products/workstation-player/workstation-player-evaluation.html).
3. Para instalar VMWare Workstation 16 y Ubuntu 18, seguir las instrucciones del siguiente link [https://shaadlife.com/install-ubuntu-using-vmware/](https://shaadlife.com/install-ubuntu-using-vmware/) o seguir el video [https://www.youtube.com/watch?v=hCKYq1nP8H0](https://www.youtube.com/watch?v=hCKYq1nP8H0). **Nota: En las guias anteriores se instala el `.iso` de Ubuntu 20.04, en nuestro caso, usamos el de Ubuntu 18.04. De ser posible, darle una memoria mínima de 60gb a su maquina virtual.**
4. Al final deben de tener algo parecido como lo de la imagen inferior:

![VMWare Linux install](/media/instalacion_final_ubuntu.png)

**Y si son barbaros y arriesgados, eliminen windows y ponganse linux de cabeza!**

## Instalación de ROS

Para instalar ROS en Ubuntu, es necesario el uso de una ventana de terminal. Las ventanas de terminal son usadas para realizar tareas y acciones que comúnmente se realizan usando interfaces gráficas. Estas incluyen la instalación de paquetes de software, librerias, etc.

Se instalará ROS Melodic (es la versión especifica para Ubuntu 18.04).

Una terminal se puede abrir usando `Ctrl+Alt+T` o también desde el menú de aplicaciones de Ubuntu. En una terminal, se debe escribir al comando a usar o la instrucción a realizar y luego dar `Enter` para ejecutar. Un ejemplo es el comando `sudo apt update` que entre sus capacidades ayuda a ver actualizaciones de programas instalados.

Seguir los siguientes pasos para la instalación de ROS:

1. Abrir una terminal.
2. Ejecutar el comando `sudo apt update`. Si se les pide una contraseña de usuario, ingresarla y dar `Enter`. Leer lo que la termina expresa, en ocasiones puede pedir alguna confirmación para lo cual se debe presionar `S` o `Y` dependiendo del idioma.
3. Ejecutar el comando `sudo apt upgrade`.
4. Para finalmente instalar ROS, ejecutar los siguientes comandos en orden:

   > sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

   > sudo apt install curl # if you haven't already installed curl

   > curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

   > sudo apt update

   > sudo apt install ros-melodic-desktop-full

   > echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

   > source ~/.bashrc

   > sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

   > sudo apt install python-rosdep

   > sudo rosdep init

   > rosdep update

5. Si ningun error ocurre, ROS debe estar instalado correctamente. Abrir una terminal y correr `roscore`. Se deberá obtener los siguiente.

![instalacion_ros](/media/instalacion_ros.png)
