# Agregando sensores y movimiento al robot

En este caso, para agregar los sensores y el movimiento del robot, usaremos plugins de Gazebo.

## Agregar un Lidar simulado

Para agregar en el simulación un lidar, primero debemos definir un eslabon en nuestro URDF donde estaría el lidar. Esto se puede hacer con los siguientes pasos:

1. Abrir el archivo URDF del robot y agregar el siguiente código al final:

   ```xml
   <link name="laser_link">
       <visual>
           <origin xyz="0 0 0" rpy="0 0 0" />
           <geometry>
               <box size="0.1 0.1 0.07"/>
           </geometry>
       </visual>
   </link>

   <joint name="laser_joint" type="fixed">
       <origin xyz="0 0 0.02" rpy="0 0 0" />
       <parent link="base_footprint" />
       <child link="laser_link" />
       <!-- <axis xyz="0 0 1"/>
       <limit effort="1" velocity="20" />
       <joint_properties friction="0.0"/> -->
   </joint>
   ```

2. Ahora agregar el plugin de lidar de Gazebo:

   ```xml
   <!-- lidar -->
   <gazebo reference="laser_link">
       <material>Gazebo/SkyBlue</material>
       <sensor type="ray" name="lds_lfcd_sensor">
           <pose>0 0 0 0 0 0</pose>
           <visualize>false</visualize>
           <update_rate>5.5</update_rate>
           <ray>
               <scan>
                   <horizontal>
                       <samples>360</samples>
                       <resolution>1</resolution>
                       <min_angle>0.0</min_angle>
                       <max_angle>6.28319</max_angle>
                   </horizontal>
               </scan>
               <range>
                   <min>0.15</min>
                   <max>12</max>
                   <resolution>0.015</resolution>
               </range>
               <noise>
                   <type>gaussian</type>
                   <mean>0.0</mean>
                   <stddev>0.01</stddev>
               </noise>
           </ray>
           <plugin name="gazebo_ros_lds_lfcd_controller" filename="libgazebo_ros_laser.so">
               <topicName>scan</topicName>
               <frameName>laser_link</frameName>
           </plugin>
       </sensor>
   </gazebo>
   ```

   Ahora cuando simulemos, podremos ver en RViz los datos del lidar.

## Plugin de movimiento

Para el movimiento, usaremos un plugin de robot diferencial de Gazebo, seguir los pasos:

1. Modificar el archivo URDF y agregar el siguiente código:

```xml
<!-- diff drive -->
    <gazebo>
        <plugin name="turtlebot3_burger_controller" filename="libgazebo_ros_diff_drive.so">
            <commandTopic>cmd_vel</commandTopic>
            <odometryTopic>odom</odometryTopic>
            <odometryFrame>odom</odometryFrame>
            <odometrySource>world</odometrySource>
            <publishOdomTF>true</publishOdomTF>
            <robotBaseFrame>base_footprint</robotBaseFrame>
            <publishWheelTF>false</publishWheelTF>
            <publishTf>true</publishTf>
            <publishWheelJointState>true</publishWheelJointState>
            <legacyMode>false</legacyMode>
            <updateRate>30</updateRate>
            <leftJoint>link2</leftJoint>
            <rightJoint>link3</rightJoint>
            <wheelSeparation>0.3902</wheelSeparation>
            <wheelDiameter>0.156</wheelDiameter>
            <wheelAcceleration>1</wheelAcceleration>
            <wheelTorque>9.6</wheelTorque>
            <rosDebugLevel>na</rosDebugLevel>
        </plugin>
    </gazebo>
```

Ahora si, a simular el robot. Hasta el punto actual, el robot puede moverse y conocer su entorno pero todavia se pueden agregar más sensores y funcionalidades, ahora vienen los retos.

## Retos finales

### Agregar una cámara simulada

Usar el siguiente código:

```xml
<!-- depth camera -->
    <gazebo reference="d435_link">
        <material>Gazebo/Indigo</material>
        <sensor type="depth" name="realsense_R200">
            <always_on>true</always_on>
            <visualize>false</visualize>
            <camera>
                <horizontal_fov>1.5184</horizontal_fov>
                <image>
                    <width>470</width>
                    <height>270</height>
                    <format>R8G8B8</format>
                </image>
                <depth_camera>
                </depth_camera>
                <clip>
                    <near>0.03</near>
                    <far>4</far>
                </clip>
            </camera>
            <plugin name="camera_controller" filename="libgazebo_ros_openni_kinect.so">
                <baseline>0.2</baseline>
                <alwaysOn>true</alwaysOn>
                <updateRate>15.0</updateRate>
                <cameraName>d435</cameraName>
                <frameName>depth_camera_link</frameName>
                <imageTopicName>rgb/image_raw</imageTopicName>
                <depthImageTopicName>depth/image_raw</depthImageTopicName>
                <pointCloudTopicName>depth/points</pointCloudTopicName>
                <cameraInfoTopicName>rgb/camera_info</cameraInfoTopicName>
                <depthImageCameraInfoTopicName>depth/camera_info</depthImageCameraInfoTopicName>
                <pointCloudCutoff>0.4</pointCloudCutoff>
                <hackBaseline>0.07</hackBaseline>
                <distortionK1>0.0</distortionK1>
                <distortionK2>0.0</distortionK2>
                <distortionK3>0.0</distortionK3>
                <distortionT1>0.0</distortionT1>
                <distortionT2>0.0</distortionT2>
                <CxPrime>0.0</CxPrime>
                <Cx>0.0</Cx>
                <Cy>0.0</Cy>
                <focalLength>0</focalLength>
                <hackBaseline>0</hackBaseline>
            </plugin>
        </sensor>
    </gazebo>
```

### Mover el robot

Este reto trata, sobre hacer un nodo para mover el robot. El robot debe de recibir sus velocidades en el topic `cmd_vel`. Deberán identificar el tipo de mensaje que se usa y desde un nodo en python enviar las velocidades deseadas a ese topic.
