## 🖥️ Crear visualización

- Hacer **CD** sobre la sección `VISUs`.

- Seleccionar `Add → Visualization` y pulsar en **Open** en la ventana *popup*.

    ![Imagen](../images/01_conceptos/image%2013.png){width=240px}

- En la parte derecha de la pantalla aparecerá la sección `Toolbox` donde, en la sección `Basic` aparecen las formas básicas. Arrastrar a la visualización los elementos que se quieran.

!!! warning "Importante"
    Si no aparece la sección, mostrarlo entrando en el **Menú** `View → Toolbox`

!!! note "Recomendación"
    Se recomienda utilizar **rectángulos** para crear botones tanto para las entradas como para las salidas.

### Botones para variables binarias

- Dibujar un rectángulo con el tamaño deseado.

- Escribir dentro la etiqueta que queramos que aparezca en el botón.

- Introducir la variable de tipo `BOOL` que queremos asociar a dicho botón. Dependiendo del comportamiento que queramos que tenga el botón, esta variable se introduce en una sección distinta dentro de `Properties → Input Configuration` (la pestaña `Properties` aparece a la derecha, normalmente combinada con `Toolbox`).

    - Si queremos que la variable cambie de valor **mientras** se pulsa el botón con el ratón pero vuelva a su valor anterior una vez soltado el ratón, introduciremos la variable en la sección `Tap`:

        ![Imagen](../images/01_conceptos/image%2014.png){width=240px}

    - Si queremos que la variable cambie de valor cada vez que pulsemos el botón lo introduciremos en la sección `Toggle` (el valor conmutará entre `TRUE` y `FALSE`):

        ![Imagen](../images/01_conceptos/image%2015.png){width=240px}

### Rectángulos para valores numéricos

- Dibujar un rectángulo con el tamaño deseado.

- Escribir dentro el *placeholder* correspondiente según el tipo de valor numérico que queremos introducir:
  
    - `%d` para variables enteras: `INT`, `UINT`.
    - `%.2f` para variables reales/flotantes: `FLOAT`, `REAL` (en este caso con dos decimales).
    - `%s` para variables de tiempo `TIME` con un formato estándar, ej: `T#2s`.
    - `%t[mm:ss]` para variables de tiempo `TIME` con un formato extendido en minutos y segundos, ej: `01:30` (1 minuto y 30 segundos).

- Escribir dentro de la propiedad `Text Variables` el nombre de la variable que queremos asociar a este rectángulo (ej. `MAIN.ManiobrasSolicitadas`).

- Si además de visualizar el valor queremos tener la opción de **cambiar su valor durante la ejecución del programa**, deberemos añadir un comportamiento al pulsar el botón del ratón:

    - En la sección `InputConfiguration`, hacer clic sobre `OnMouseClick > Configure` y seleccionar `Write a Variable`. Dejando los parámetros por defecto en la ventana *popup* que se muestra, lo que se escriba en este rectángulo **se asignará a la variable especificada en** `Text Variables`.

        ![Imagen](../images/01_conceptos/visu_write_variable.png){width=600px}

### Usar imágenes en la visualización

- Crear una carpeta en el proyecto PLC para las imágenes: **CD** sobre el proyecto y pulsar `Add → New Folder...` y llamarlo `_resources` (por ejemplo).

    ![Imagen](../images/01_conceptos/add_folder.png){width=450px}

- Arrastrar desde el explorador de Windows hasta la carpeta `_resources` las imágenes que se quieran utilizar en la visualización.

- Añadir una lista de imágenes (*Image Pool*): **CD** sobre el proyecto `Add → Image Pool...` y darle un nombre (`IP_Carro` en la imagen).

    ![Imagen](../images/01_conceptos/image_pool_add.png){width=450px}

- Añadir las imágenes al listado: Doble clic sobre `IP_Carro` y **CI** en los tres puntos sobre la columna `File name` para buscar el fichero de imagen que queramos añadir al listado. Escribir un `ID` en la primera columna para poder referirnos a la imagen posteriormente (`LOGO_UMA` en la imagen de ejemplo).

    ![Imagen](../images/01_conceptos/image_pool_add_image.png){width=350px}

- Insertar una imagen en la visualización: Arrastrar un objeto de tipo `Image` desde la *Toolbox* (sección `Basic`) hasta la visualización.

- Escribir el `ID` de la imagen a utilizar en la propiedad `Static ID` del objeto, por ejemplo: `IP_Carro.LOGO_UMA`.

    ![Imagen](../images/01_conceptos/image_pool_static_id.png){width=350px}

!!! warning "Importante"
    Nótese que se necesita especificar la `ID` completa, incluyendo el nombre de la *Image Pool*.
