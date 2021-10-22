# ROS Aplicado

Para esta sesión, diseñaremos un pequeño robot móvil diferencal en [OnShape](https://www.onshape.com/en/) y lo pasaremos a ROS para simularlo y probar su movimiento.

**Vamo a darle!**

## Diseño de robot

Primero necesitamos hacernos una cuenta en OnShape y generar un proyecto, para ello seguir los siguientes pasos:

1. Crearse una cuenta en la página [OnShape](https://www.onshape.com/en/).
2. Ingresar a su cuenta en la página principal y deben ver lo siguiente:

   ![onshape_main_page](/media/onshape_main_page.png)

3. Crear un nuevo documento:

   ![onshape_new_doc](/media/onshape_new_doc.png)

4. Ingresar nombre del documento y dar `ok`.

   ![onshape_input_name](/media/onshape_input_name.png)

5. Cambiar las unidades a milimetros.

   ![onshape_units](/media/onshape_units.png)

### Diseño de base del robot

1. Dar click en `sketch`.

   ![onshape_doc_planes](/media/onshape_doc_planes.png)

2. Escoger el plano top.

   ![onshape_sketch_top](/media/onshape_sketch_top.png)

3. Usar el rectángulo central y dibujar un rectángulo en el plano top:

   ![onshape_sketch_base](/media/onshape_sketch_base.png)

4. Usar la herramienta de extruir para formar la base.

   ![onshape_sketch_base_extrude](/media/onshape_sketch_base_extrude.png)

   ![onshape_sketch_base_extrude_2](/media/onshape_sketch_base_extrude_2.png)

5. En los extremos de la base, dibujar circulo con `sketch` y extruirlo como un cilindro.

   ![onshape_sketch_base_eje](/media/onshape_sketch_eje.png)

   ![onshape_sketch_base_eje_3d](/media/onshape_sketch_eje_3d.png)

6. Realizar lo mismo del otro lado, se puede usar la opción `mirror`.
   ![onshape_sketch_base_eje_mirror](/media/onshape_sketch_eje_mirror.png)

Ya tenemos la base del robot diseñada, ahora faltan las llantas y la rueda loca.

### Diseño de ruedas del robot

1. Crear una nueva parte del robot.

   ![onshape_new_llantas](/media/onshape_new_llantas.png)

2. Usando `sketch` y `extrude`, generar una rueda en el plano top:

   ![onshape_sketch_llanta](/media/onshape_sketch_llanta.png)

   ![onshape_llanta_3d](/media/onshape_llanta_3d.png)

**Y ya tenemos nuestras llantas asi de simple!**

Ahora hace falta la rueda loca.

### Diseño de rueda loca

Ya saben que hacer. Seguir los pasos:

1. Crear una nueva parte.

2. Realizar un sketch en el plano frontal como el siguiente:

   ![onshape_sketch_rueda_loca](/media/onshape_sketch_rueda_loca.png)

3. Usaremos la herramienta `revolve` para formar una media esfera.

   ![onshape_3d_rueda_loca](/media/onshape_3d_rueda_loca.png)

Tenemos nuestra rueda loca! Ahora a ensamblar nuestro pequeño robot.

## Ensamblaje del robot

Seguir los pasos para el ensamble:

1. Abrir la pestaña ensamble:

   ![onshape_ensamble](/media/onshape_ensamble.png)

2. Agregar con `Insert` las partes diseñadas:

   ![onshape_ensamble_partes](/media/onshape_ensamble_partes.png)

3. Unir las llantas a la base del robot usando revolute. Hacerlo para ambas llantas.

   ![onshape_ensamble_revolute](/media/onshape_ensamble_revolute.png)

   ![onshape_ensamble_revolute](/media/onshape_ensamble_revolute_2.png)

4. Unir la rueda loca usando fastened:

   ![onshape_ensamble_fastened](/media/onshape_ensamble_fastened.png)

   ![onshape_ensamble_fastened](/media/onshape_ensamble_fastened_2.png)

5. Cambiar los nombres de las juntas con el formato `dof_link<numero>`:

   ![onshape_dof_rename](/media/onshape_dof_rename.png)

6. Asignar el material a cada una de las partes.

   ![onshape_material_assign](/media/onshape_material_assign.png)

Ahora si tenemos el diseño listo para comenzar a pasarlo a ROS. En la guia [Cómo pasar de OnShape a ROS](./ONSHAPE_TO_ROS.md).
