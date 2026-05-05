## 📝 Convenciones de nombres

Se recomienda llamar a todos los elementos del proyecto con el nombre adecuado **desde el principio**, ya que renombrar *a posteriori* puede acarrear problemas derivados del acceso a elementos cuya ruta ha cambiado. No obstante, si es necesario renombrar los elementos, el procedimiento es como sigue.

### Soluciones

Se recomienda llamar a las soluciones de TC3 de la misma forma que los Proyectos de TC3.

Para renombrarlo una vez creado:

  1.  **CD** sobre el nombre de la solución y seleccionar ***Rename***.

    ![Imagen](../images/01_conceptos/image%2018.png){width=240px}

  1.  Escribir el nuevo nombre.

### Proyectos TwinCAT3

- Se recomienda llamar a los proyectos TC3 de la siguiente forma:
    - **Para los ejemplos**: `TC3_[nombre]`  
      donde el nombre debe ser algo significativo.

        !!! question "Ejemplo"
            `TC3_Lampara`

            `TC3_Carro`

    - **Para los trabajos finales**: `[codigo]_TC3_G[grupo]`  
      donde `codigo` debe escogerse según la asignatura (`AIM`, `AIE`, `SR`, etc.) y `grupo` debe ser el número del grupo con dos dígitos (`01`, `02`, ...).

        !!! question "Ejemplo"
            `AIM_TC3_G01`

            `SR_TC3_G12`

    - Códigos de las asignaturas:

        | Código | Asignatura                                      |
        |--------|-------------------------------------------------|
        | AIM    | Automatización Industrial de GIERM              |
        | AIE    | Automatización Industrial de GIEI o GIEI+IEL    |
        | SR     | Sistemas Robotizados de GITI                    |

    - Para renombrarlo una vez creado:
        - **CD** sobre el nombre del proyecto TC3 y seleccionar *Rename*.

            ![Imagen](../images/01_conceptos/image%2019.png){width=240px}

        - Escribir el nuevo nombre.

### Proyectos PLC

- Se recomienda llamar a los proyectos PLC de la forma `[nombre]_PLC` para los ejemplos o `[estacion]_[nivel]_PLC` para los trabajos, donde el nombre debe ser algo significativo.

    !!! question "Ejemplo"
        `Lampara_PLC`

        `FMS201_Monolitico_PLC`

- Para renombrarlo una vez creado:
    1. **CD** sobre el nombre del proyecto PLC y seleccionar *Rename*.

        ![Imagen](../images/01_conceptos/image%2020.png){width=240px}

    2. Escribir el nuevo nombre.
    3. Aparecerá un aviso indicando que si se cambia el nombre del proyecto no se van a poder hacer cambios online (en caso de que se esté ejecutando). **CI** en Sí.

        ![Imagen](../images/01_conceptos/image%2021.png){width=432px}

    4. Tras unos segundos, el proyecto PLC habrá cambiado de nombre.
    5. **Importante:** Puede ocurrir que, tras el cambio de nombre, al hacer **CI** sobre el proyecto, salga un aviso de error por no encontrar el nombre anterior. Esto se debe solucionar haciendo **CI** sobre `Build → Rebuild Solution`.

### Bloques Funcionales

- Se recomienda llamar a los bloques funcionales de la forma `FB_[nombre]_[lenguaje]`, donde el nombre debe ser algo significativo.
- Los lenguajes suelen ser:
    - `ST` (*Structured Text*)
    - `SFC` (*Sequential Function Chart*)

    !!! question "Ejemplo"
        `FB_Estacion_ST`

        `FB_Coordinador_ST`
        
        `FB_Alimentador_SFC`

- Para renombrarlo una vez creado:
    1. **CD** sobre el nombre del **FB** y seleccionar ***Rename***.
    2. Escribir el nuevo nombre.
    3. Aparecerá un aviso indicando que se van a adaptar todas las referencias en el proyecto. Pulsar en ***Yes***.

        ![Imagen](../images/01_conceptos/image%2022.png){width=384px}

    4. Aparecerá una ventana mostrando todos los cambios que se van a realizar. Pulsar en **OK**.

        ![Imagen](../images/01_conceptos/image%2023.png){width=598px}

### Variables

- Se recomienda llamar a las variables con nombres significativos.
- Si las variables se van a asociar con los terminales de entrada y salida, es **obligatorio** llamarlas con el nombre indicado en la columna **Variable** de la tabla de E/S.
- Para renombrar una variable y que ese cambio se corrija en todas las referencias que se hagan a la misma en el proyecto, hay que hacer **CD** sobre el nombre de la variable y seleccionar `Refactoring → Rename`.
- Aparecerá un *popup* donde se debe indicar el nuevo nombre.

    ![Imagen](../images/01_conceptos/image%2024.png){width=240px}

- Aparecerá una ventana mostrando todos los cambios que se van a realizar. Pulsar en **OK**.

    ![Imagen](../images/01_conceptos/image%2025.png){width=626px}
