## ▶️ Ejecutar programa

El procedimiento para ejecutar el programa implica, habitualmente, la siguiente secuencia de pasos:

- Seleccionar el controlador que queremos que ejecute el mismo. 
- Posteriormente, compilaremos el proyecto y activaremos la configuración.
- Descargaremos el programa en el controlador.
- Finalmente pondremos el programa en ejecución. 

Este proceso lo llamaremos, de manera informal, el **Ciclo Básico del Programador en TC3**.

![Imagen](../images/01_conceptos/ciclo_basico_tc3.png){width=400px}

### Seleccionar controlador

El programa puede ser ejecutado en distintos "controladores"

- Emulador `Local`.
- Simulador `Um_RT_Default` (*User mode Real Time*). ==Recomendado para el laboratorio.==
- Controlador remoto (PLC).

#### Emulador local

Para poder usar este controlador debemos haberle dejado a TwinCAT 3 que tuviera acceso al *kernel* de Windows durante la instalación, de manera que pueda hacer uso de, al menos, un *core* del equipo para ejecutar el programa. 

El emulador local ejecuta el programa exactamente de la misma manera que si lo hiciéramos en un equipo remoto pero, obviamente, no tenemos acceso al *hardware*. De esta forma, podremos interactuar con las variables de entrada y salida mediante la escritura/forzado de variables o usando la visualización (si hemos diseñado alguna para controlar las variables).

En este caso no tendremos que asociar las variables a los terminales de E/S ya que no habrá ninguno disponible.

Para usar este controlador, simplemente asegúrate de seleccionar `Local` en el desplegable del `Target`.

![Imagen](../images/01_conceptos/image%2027.png){width=200px}

!!! warning "Importante"
    La instalación y uso de este modo tiene ciertos requisitos que se cumplen en la mayoría de los equipos en los que se puede instalar, pero, en ocasiones, puede dar algún problema de incompatibilidad. Para estos casos, se recomienda utilizar el **simulador local** explicado más adelante.

#### Simulador local

TwinCAT 3 proporciona una vía alternativa al emulador local que permite ejecutar código en un simulador en **modo usuario** dentro de Windows. La diferencia principal con el emulador es que éste garantiza el tiempo de ciclo del sistema mientras que el simulador no lo hace. Aún así, las restricciones de tiempo de los programas que usaremos no son muy exigentes así que el simulador será suficiente para una ejecución satisfactoria. A cambio, elimina los problemas de compatibilidad que la instalación del emulador local pueda tener.

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

#### Controlador remoto (PLC)

Por último, podremos ejecutar nuestro programa en un controlador remoto (por ejemplo, el PLC de la estación FMS20x). De esta manera, tendremos acceso al *hardware* que esté conectado al controlador y podremos interactuar con él.

Para usar este controlador, lo seleccionaremos en el desplegable de `Target`. 
![Imagen](../images/01_conceptos/target_umrt_default.png){width=200px}

!!! warning "Importante"
    Si no aparece el controlador deseado en el listado, será necesario escanear la red para encontrar el equipo remoto y establecer la conexión con él. Para ello, seguiremos las instrucciones detalladas [aquí](../../contenidos/01_conceptos/#busqueda-en-red) y [aquí](../../contenidos/01_conceptos/#escaneado-del-controlador).

Al usar este controlador, tendremos acceso al *hardware* conectado a él, y podremos vincular las variables que hemos declarado en las imágenes de entrada y salida con los terminales y canales que queramos. Para ello, simplemente repetiremos este proceso para cada variable:

- **DCI** sobre la variable a vincular en la lista que aparece en la sección de instancia del proyecto.
    
    ![Imagen](../images/01_conceptos/image%2026.png){width=200px}

- Seleccionar el terminal/canal deseado del listado que aparece.

### Compilar programa
Una vez el programa está implementado (independientemente del lenguaje utilizado):

- Compilar el proyecto: Menú `Build → Build [nombre del proyecto]`.
- Asegurarse de que no hay errores.
- Si has declarado variables en las **imágenes de entrada y/o salida**:     
    - **Verificar** que las variables aparecen en la zona de la instancia.
        ![Imagen](../images/01_conceptos/image%2026.png){width=200px}

### Activar configuración

Una vez realizada la selección del controlador y la asociación de variables con los terminales de E/S (si procede), ahora debemos envíar esta información al controlador en cuestión. Esto se denomina **Activar la configuración**.

Para ello, deberemos pulsar el icono de ***Activate Configuration*** y activar el modo de ejecución (***Run Mode***) cuando nos lo pregunte TwinCAT3 en una ventana *popup*.

![Imagen](../images/01_conceptos/image%2028.png){width=40px}

### Transferir y ejecutar programa 

Posteriormente, debemos enviar el programa al controlador pulsando el icono de ***Login***, tras lo que se preguntará, en un *popup*, si queremos crear un puerto de comunicación con el controlador y descargar el programa. Pulsaremos en ***Yes***.

![Imagen](../images/01_conceptos/image%2029.png){width=400px}

Finalmente, pondremos el programa en ejecución pulsando el icono ***Start***.

![Imagen](../images/01_conceptos/image%2030.png){width=60px}

!!! warning "Importante"
    Para poder modificar de nuevo el programa, primero hay que parar el programa (***Stop***) (**recomendado**) y posteriormente hacer ***Logout***.

    ![Imagen](../images/01_conceptos/image%2031.png){width=60px}
    