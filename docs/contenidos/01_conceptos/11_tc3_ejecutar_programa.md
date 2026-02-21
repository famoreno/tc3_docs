## ▶️ Ejecutar programa

Una vez el programa está implementado (independientemente del lenguaje utilizado):

- Compilar el proyecto: Menú `Build → Build [nombre del proyecto]`.
- Asegurarse de que no hay errores.
- Si has declarado variables en las **imágenes de entrada y/o salida**:     
    - Comprobar que las variables aparecen en la zona de la instancia.
        
        ![Imagen](../images/01_conceptos/image%2026.png){width=200px}

- Si quieres utilizar el **Equipo Remoto**:
    - Vincular las variables con los terminales correspondientes según la tabla de E/S:
        - **DCI** sobre la variable a vincular.
        - Seleccionar el terminal/canal deseado del listado que aparece.
    - Buscar el equipo remoto.

- Si quieres usar el **Runtime local**
    - Asegurarte de que seleccionar ``Local` en el desplegable del `Target`. 
        
        ![Imagen](../images/01_conceptos/image%2027.png){width=200px}

- Activar la configuración en el `Target` (***Activate Configuration***) y activar el modo de ejecución (***Run Mode***). Esto último te lo pregunta TwinCAT3 en una ventana *popup*.
        
    ![Imagen](../images/01_conceptos/image%2028.png){width=50px}

- Descargar el programa en el `Target` (***Login***), donde se preguntará, en un *popup*, si quieres crear un puerto de comunicación con el `Target` y descargar el programa. Pulsar en ***Yes***.

    ![Imagen](../images/01_conceptos/image%2029.png){width=400px}

- Ejecutar el programa (***Start***).

    ![Imagen](../images/01_conceptos/image%2030.png){width=70px}

!!! warning "Importante"
    Para poder modificar de nuevo el programa, primero hay que parar el programa (***Stop***) [**recomendado**] y posteriormente hacer ***Logout***.

    ![Imagen](../images/01_conceptos/image%2031.png){width=70px}
    
