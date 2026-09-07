Una vez terminada la implementación del programa, procederemos a ponerlo en ejecución. El procedimiento para ejecutar el programa implica, habitualmente, la siguiente secuencia de pasos:

1.  Compilar el proyecto.
2.  Seleccionar el controlador que queremos que ejecute el mismo.
3.  Activar la configuración.
4.  Descargar el programa en el controlador.
5.  Finalmente, poner el programa en ejecución.

Este proceso lo llamaremos, de manera informal, el **Ciclo Básico del Programador en TC3**, que explicaremos paso por paso.

![Imagen](../images/01_conceptos/ciclo_basico_tc3.png){width=500px}

## 1 > Compilar programa

Una vez el programa está implementado (independientemente del lenguaje utilizado):

- Compilar el proyecto: Menú `Build → Build [nombre del proyecto]`.
- Asegurarse de que no hay errores.
- Si has declarado variables en las **imágenes de entrada y/o salida**:

    !!! tip "Ejemplo"
        ```pascal
        PROGRAM MAIN
        VAR
            Pulsador AT %I*: BOOL;
            Lampara AT %Q*: BOOL; 
        END_VAR
        ```

    - **Verificar** que las variables aparecen en las secciones `PlcTask Inputs` y `PlcTask Outputs` en la zona de la instancia del proyecto.

        ![Imagen](../images/01_conceptos/image%2026.png){width=200px}

## 2 > Seleccionar controlador destino

En TwinCAT 3, el programa puede ser ejecutado en distintos "controladores"

- Emulador `Local`.
- Simulador `Um_RT_Default` (*User mode Real Time*). ==Recomendado para el laboratorio.==
- Controlador remoto (PLC).

### Emulador (local)

Para poder usar este controlador debemos haberle dejado a TwinCAT 3 que tuviera acceso al *kernel* de Windows durante la instalación, de manera que pueda hacer uso de, al menos, un *core* del equipo para ejecutar el programa.

El emulador local ejecuta el programa exactamente de la misma manera que si lo hiciéramos en un equipo remoto pero, obviamente, no tenemos acceso al *hardware*. De esta forma, podremos interactuar con las variables de entrada y salida mediante la escritura/forzado de variables o usando la visualización (si hemos diseñado alguna para controlar las variables).

En este caso no tendremos que asociar las variables a los terminales de E/S, ya que no habrá ninguno disponible.

Para usar este controlador, simplemente asegúrate de seleccionar `Local` en el desplegable del `Target`.

![Imagen](../images/01_conceptos/image%2027.png){width=200px}

!!! warning "Importante"
    La instalación y uso de este modo tiene ciertos requisitos que se cumplen en la mayoría de los equipos en los que se puede instalar, pero, en ocasiones, puede dar algún problema de incompatibilidad. Para estos casos, se recomienda utilizar el **simulador local** explicado más adelante.

### Simulador (Um_RT_Default)

!!! info "Información"
    Este será el controlador a utilizar en las prácticas **cuando no se tenga acceso a las estaciones**.

TwinCAT 3 proporciona una vía alternativa al emulador local que permite ejecutar código en un simulador en **modo usuario** dentro de Windows. La diferencia principal con el emulador es que este garantiza el tiempo de ciclo del sistema mientras que el simulador no lo hace. Aún así, las restricciones de tiempo de los programas que usaremos no son muy exigentes así que el simulador será suficiente para una ejecución satisfactoria. A cambio, elimina los problemas de compatibilidad que la instalación del emulador local pueda tener.

Al igual que con el emulador local, no tendremos que asociar las variables a los terminales de E/S, ya que no habrá ninguno disponible. De nuevo, podremos interactuar con las variables de entrada y salida mediante la escritura/forzado de variables o usando la visualización (si hemos diseñado alguna para controlar las variables).

Para usar este controlador, tendremos que ejecutar en "**modo Administrador**" el archivo `TC3_UmRT_Start.bat` que proporcionamos en la carpeta `Automatización > programas` del CV (alternativamente, hay una copia de este fichero en la carpeta `C:\TwinCAT\3.1\Runtimes\UmRT_Default`). Esto abrirá un terminal de Windows con la información relativa a la ejecución del simulador. Minimizaremos esta ventana y **la dejaremos trabajar de fondo**.

!!! danger "Problema en el laboratorio"
    Se ha detectado que este procedimiento no funciona en los PCs del laboratorio por lo que, alternativamente, hay que realizar lo siguiente:

    Pulsar `Win+R` e introducir el siguiente texto: 
    `C:\TwinCAT\3.1\Runtimes\UmRT_Default\Start.bat`

!!! warning "Importante"
    No debemos cerrar la ventana del terminal de Windows abierto por `TC3_UmRT_Start.bat` mientras queramos usar este simulador.

Una vez hecho esto, aparecerá el texto `UmRT_Default` en el desplegable del `target`:
![Imagen](../images/01_conceptos/target_umrt_default.png){width=200px}

!!! warning "Importante"
    Una vez finalizado nuestro trabajo con el simulador, pulsaremos la tecla `'x'` en el terminal para apagar el simulador y la ventana se cerrará automáticamente.

### Controlador remoto (PLC)

!!! info "Información"
    Este será el controlador que utilizaremos en las prácticas **cuando se tenga acceso a las estaciones**.

Por último, podremos ejecutar nuestro programa en un controlador remoto (por ejemplo, el PLC de la estación FMS20x). De esta manera, tendremos acceso al *hardware* que esté conectado al controlador y podremos 
interactuar con él.

Para usar este controlador, lo seleccionaremos en el desplegable de `Target`.

![Imagen](../images/01_conceptos/target_umrt_default.png){width=200px}

En este ejemplo, el controlador sería el mostrado como `CX-840BC5`, pero, en las prácticas, deberán aparecer los nombres de las estaciones (ej. `FMS-201`). Si no aparece el controlador deseado en el listado, o se ha perdido la conexión con él, será necesario **escanear la red** para encontrar el equipo remoto y establecer una nueva conexión.

#### Búsqueda de controladores remotos

!!! tip "Recomendación"
    Hay un video de ejemplo en el Campus Virtual en `Automatización > Videos > TC3` con nombre `9_Runtime_Target_*.mkv`.

Para buscar controladores remotos en la red local del laboratorio, hay que seguir el siguiente procedimiento.

1. Desplegar `Target` y seleccionar `Choose Target System...`.

    ![Imagen](../images/01_conceptos/target_umrt_default.png){width=150px}

    ![Imagen](../images/01_conceptos/choose_target_system.png){width=400px}

2. Pulsar sobre `Search (Ethernet)...`

    !!! warning "Importante"
        Aceptar el siguiente mensaje: *Searching for remote system only possible from local system. Change back to local system*, si aparece.

    1. Se abre el siguiente cuadro de diálogo, donde hay que seleccionar `🔳 Advanced Settings`.:

        ![Imagen](../images/01_conceptos/add_route_dialog.png){width=400px}

    2. Pulsar sobre el botón `Broadcast Search`.
    3. Seleccionar los adaptadores de red en el *popup* que aparece y pulsar `OK`.

        ![Imagen](../images/01_conceptos/select_adapter.png){width=400px}

    4. Seleccionar el PLC deseado de la lista que aparezca.
    5. Marcar `🔳 IP Address`. <span class="fondo-amarillo">**Importante**</span>
    6. Pulsar en el botón `Add Route`.
    7. En el *popup* que aparece (***Add Remote Route***):
        - Desmarcar `🔲 Secure ADS` si está seleccionado. <span class="fondo-amarillo">**Importante**</span>
        - Dejar los campos `User` y `Password` tal y como están.

        <!-- 
        - Escribir: `User` el que corresponda (Administrator, por defecto).
        - Escribir: `Password` la que corresponda (1, por defecto). 
        -->

    8. Observar que aparece una `x` en la columna *Connected*.
    9. Cerrar el cuadro de diálogo pulsando `Close`.

3. Ahora debería aparecer el controlador en el listado del cuadro de diálogo `Choose Target System`:
    - Seleccionar el controlador en la lista y pulsar `OK`.

4. Si aparece un *popup* indicando que es necesario cambiar la plataforma, pulsar en `Yes`.

#### Escaneado de terminales

La **primera vez** que conectemos con un controlador remoto, deberemos realizar un escaneado de sus módulos para determinar los terminales de entrada y salida disponibles en los mismos. El procedimiento es el siguiente.

!!! warning "Importante"
    Para poder hacer este proceso, debemos asegurarnos que TwinCAT 3 está en modo configuración (*Configuration Mode*) y no en ejecución (*Run Mode*).

    ![Imagen](../images/01_conceptos/tc3_modes.png){width=150px}

- En el explorador de la solución (`Solution Explorer`):
    - Seleccionar: `I/O > Devices`.
    - Pulsar en el menú `TwinCAT > Scan (BD Scan)` (alternativamente, **CD** sobre `I/O Devices` y pulsar `Scan`).
    - Aceptar el mensaje de que no todos los dispositivos pueden encontrarse automáticamente.
    - Seleccionar únicamente el dispositivo `EtherCAT` y pulsar `OK`.
    - Aceptar la búsqueda de *boxes* (terminales) pulsando `Yes`.
    - Aceptar la activación del modo *Free Run* pulsando `Yes`.

- Observar el árbol de I/O en el explorador de la solución.
- Desplegar el elemento `EK1200` y verificar que la lista de terminales se corresponde con la configuración del controlador (en su documentación).

    !!! info "Info"
        Tened en cuenta que el terminal `EL9011` es un elemento virtual.

#### Comprobación de los terminales

**Tras escanear los terminales por primera vez**, es una buena práctica comprobar algunos de los terminales de E/S para asegurarnos de que tenemos acceso a ellos.
Para ello, usaremos un ejemplo en el que tendremos un programa con una variable `i_PulsadorMarcha` en la imagen de entrada y una variable `o_LamparaMarcha` en la imagen de salida.

```pascal
PROGRAM MAIN
VAR
    i_PulsadorMarcha AT %I*: BOOL;
    o_LamparaMarcha AT %Q*: BOOL;
END_VAR
```

El procedimiento a seguir es el siguiente:

- Buscar en el listado de E/S del controlador **que hay en su descripción funcional** la entrada correspondiente al pulsador de marcha:
    - Localizar el **Terminal** y el **Canal** de entrada especificado.
    - Desplegar el contenido del **Canal** y hacer **DC** sobre `Input`.
    - Seleccionar la pestaña *Online* y verificar que se corresponde con el pulsador:
        - Accionar el pulsador de marcha y observar el cambio de valor mostrado en la gráfica.

    !!! warning "Importante"
        Se recomienda seleccionar la pestaña *Variable* y **cambiar el nombre** de `Input` por el nombre de la variable asociada en el listado de E/S (en este ejemplo: `i_PulsadorMarcha`).

        Este paso **NO** vincula el terminal/canal con la variable sino que simplemente lo **renombra** para ayudarnos a localizarlo posteriormente durante el proceso de vinculación.

- Buscar en el listado de E/S del controlador **que hay en su descripción funcional** la salida correspondiente a la lámpara de marcha:
    - Localizar el **Terminal** y el **Canal** de salida especificado.
    - Desplegar el contenido del **Canal** y hacer **DC** sobre su `Output`.
    - Seleccionar la pestaña *Online* y verificar que se corresponde con la lámpara:
        - Pulsar `Write`.
        - Pulsar alternativamente `1`/`0` y comprobar que la lámpara se enciende y se apaga.

    !!! warning "Importante"
        Se recomienda seleccionar la pestaña *Variable* y **cambiar el nombre** `Output` por el nombre de la variable asociada en el listado de E/S (en este ejemplo: `o_LamparaMarcha`).

        Este paso **NO** vincula el terminal/canal con la variable sino que simplemente lo **renombra** para ayudarnos a localizarlo posteriormente durante el proceso de vinculación.

Una vez realizado esto, guardamos el proyecto.

#### Vinculación de variables y E/S

!!! tip "Recomendación"
    Hay un video de ejemplo en el Campus Virtual en `Automatización > Videos > TC3` con nombre `9_Runtime_Target_*.mkv`.

Una vez se han revisado y renombrados los terminales/canales de E/S del controlador, procedemos a vincularlos con las variables de nuestro programa.

El procedimiento es el siguiente (siguiendo con el ejemplo anterior de pulsador/lámpara):

- **DC** sobre el canal nombrado como `o_LamparaMarcha` (alternativamente `CD > Change Link...`):
    - Seleccionar la variable que queremos vincular en la sección `PLCTask Output` y pulsar `OK`: `MAIN.o_LamparaMarcha`.
    - Observar cómo cambian los iconos del canal `o_LamparaMarcha` y de la variable `MAIN.o_LamparaMarcha` en la instancia (aparece una flecha sobre el icono en ambos casos, indicando que hay una vinculación).

- De manera similar, **DC** sobre la variable `MAIN.i_PulsadorMarcha` en la sección `PLCTask Input` (alternativamente `CD > Change Link...`)
    - Seleccionar en I/O el canal renombrado a `i_PulsadorMarcha`
    - De la misma manera, cambian los iconos del canal `o_LamparaMarcha` y de la variable `MAIN.o_LamparaMarcha` en la instancia.

!!! info "Info"
    Nótese que esta operación se puede hacer desde la instancia hacia la entrada/salida o al revés.


!!! info "Info"
    Esta operación solo hay que hacerla **cuando tengamos nuevas variables declaradas en las imágenes de entrada o de salida**. Una vez realizada la vinculación, la guardaremos y la activaremos siempre que vayamos a trabajar con nuestro programa.


<!-- 
!!! warning "Importante"
    Si no aparece el controlador deseado en el listado, será necesario escanear la red para encontrar el equipo remoto y establecer la conexión con él. Para ello, seguiremos las instrucciones detalladas [aquí](../../contenidos/01_conceptos/#busqueda-en-red) y [aquí](../../contenidos/01_conceptos/#escaneado-del-controlador).

Al usar este controlador, tendremos acceso al *hardware* conectado a él, y podremos vincular las variables que hemos declarado en las imágenes de entrada y salida con los terminales y canales que queramos. Para ello, simplemente repetiremos este proceso para cada variable:

- **DCI** sobre la variable a vincular en la lista que aparece en la sección de instancia del proyecto.

    ![Imagen](../images/01_conceptos/image%2026.png){width=200px}

- Seleccionar el terminal/canal deseado del listado que aparece.

!-->

## 3 > Activar configuración

Una vez realizada la selección del controlador y la vinculación de variables con los terminales de E/S (si procede), ahora debemos enviar esta información al controlador en cuestión. Esto se denomina **Activar la configuración**.

Para ello, deberemos pulsar el icono de ***Activate Configuration*** y activar el modo de ejecución (***Run Mode***) cuando nos lo pregunte TwinCAT 3 en una ventana *popup*.

![Imagen](../images/01_conceptos/image%2028.png){width=40px}

## 4 > Transferir programa

Posteriormente, debemos enviar el programa al controlador pulsando el icono de ***Login***, tras lo que se preguntará, en un *popup*, si queremos crear un puerto de comunicación con el controlador y descargar el programa. Pulsaremos en ***Yes***.

![Imagen](../images/01_conceptos/image%2029.png){width=400px}

## 5 > Ejecutar programa

Finalmente, pondremos el programa en ejecución pulsando el icono ***Start***.

![Imagen](../images/01_conceptos/image%2030.png){width=60px}

!!! warning "Importante"
    Para poder modificar de nuevo el programa, primero hay que parar el programa (***Stop***) (**recomendado**) y posteriormente hacer ***Logout***. 

    ![Imagen](../images/01_conceptos/image%2031.png){width=60px}
    
    Si no se hace ***Logout*** sin pulsar antes ***Stop***, el programa seguirá siendo ejecutado en el controlador sin que podamos ver lo que esté ocurriendo en TwinCAT 3. Durante las prácticas, **no es conveniente que suceda esto**, ya que perderemos control sobre lo que está ocurriendo con un programa que aún está en depuración.