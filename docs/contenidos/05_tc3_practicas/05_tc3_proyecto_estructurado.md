# 🗂️ Proyecto Estructurado

!!! info "Nota"
    Esta práctica es **entregable**.

- Esta sección describe un enfoque **modular** para estructurar proyectos de automatización en TwinCAT 3, utilizando *Function Blocks* (**FBs**) basados en *Sequential Function Chart* (`SFC`).
- Beneficios del Enfoque Modular:
    - **Claridad:** Cada **FB** realiza una tarea específica (ej. Cargar Base, Prensar Rodamiento).
    - **Reutilización:** Los **FBs** de tareas pueden ser reutilizados en otras estaciones o proyectos con lógica similar.
    - **Mantenimiento:** Los cambios se localizan en **FBs** específicos, reduciendo el riesgo de efectos secundarios.
    - **Testabilidad:** Cada **FB** puede probarse individualmente (si se diseña adecuadamente).
    - **Colaboración:** Diferentes desarrolladores pueden trabajar en distintos **FBs** simultáneamente.

## Ejemplo de apoyo

Utilice el ejemplo del [**carro extendido estructurado**](../04_tc3_carro_extendido/04_tc3_carro_extendido_estructurado.md) como apoyo para entender la estructura del proyecto.

## Entregables

- GRAFCET del proyecto estructurado en formato PDF.
- Programa TC3 con la lógica de control del proyecto en formato `.tnzip`.

!!! warning "Normas de entrega"
    Siga las instrucciones en [este documento](../../pdfs/0e_proyecto_automatizacion.pdf){target="_blank"} referidas al nombre del entregable.

## Funcionalidades

!!! warning "Atención"
    📃 Versión descargable [aquí](../../pdfs/Checklist_Func_Estructurado.pdf){target="_blank"}.

El proyecto desarrollado debe tener implementar las siguientes funcionalidades (en <font color="red">rojo</font>, funcionalidades nuevas respecto a la versión monolítica):

!!! info "Requeridas"
    1. Evaluación de las condiciones iniciales.
    2. Evaluación de las condiciones de marcha.
    3. Secuencia automática de restauración de las condiciones iniciales.
    4. Secuencia de producción normal.
    5. Tratamiento mejorado de la falta de material <font color="red">(en dos etapas: **Silenciar** y **Continuar**)</font>.
    6. Visualización funcional.
    7. Modo manual <font color="red">correcto y adecuado</font> (mandos directos) y automático.
    8. Modo de procesamiento por lotes (tarea).
    9. Modo ciclo a ciclo.
    10. <font color="red">Solicitud de parada a final de ciclo.</font>
    11. Gestión de la tarea (maniobras solicitadas, pendientes y realizadas).
    12. Parametrización de todas las variables (tiempos y cantidades).
    13. Reinicio y pausa del estado.

??? info "Opcionales"
    1.  Normalización y escalado de las señales de entrada analógicas.
    2.  Secuencias paralelas.
    3.  <font color="red">Señalización mejorada:</font>
        1.  <font color="red">Lámpara de alarma y lámpara de falta material intermitente en falta material.</font>
        2.  <font color="red">Lámpara de parada intermitente para indicar parada pedida.</font>

## Componentes

- `MAIN`: programa principal (`ST`)
    - `FB_Estacion`: contenedor principal (`ST`)
        - `FB_EstacionDirector_SFC`: implementa el gestor de modos del sistema (`SFC`).
        - `FB_EstacionCoordinador_SFC`: implementa el grafcet coordinador de tareas (`SFC`).
        - `FB_*_SFC`: implementa la lógica de control de la tarea; un bloque funcional por cada tarea (ej.: `FB_SituarPale`) (`SFC`).
- `VISU_Estacion`: interfaz gráfica

## Itinerario

!!! warning "Atención"
    Punto de partida: **proyecto de automatización monolítico**.

!!! warning "Atención"
    La lista es *clickable* pero **NO** guarda el estado; si actualizas o entras/sales de la página se perderán las marcas.

    📃 Versión descargable [aquí](../../pdfs/Checklist_Itinerario_Proyecto_Estructurado.pdf){target="_blank"}.

- [ ] Renombrar `FB_Estacion` por `_FB_Estacion_Monolitico`.
- [ ] Crear un nuevo `FB_Estacion` en lenguaje `ST`.
- [ ] Trasladar toda la parte de declaración de `_FB_Estacion_Monolitico` a `FB_Estacion`.
- [ ] Eliminar la invocación al proceso (`MAIN`).
- [ ] Eliminar la gestión del modo manual (`MAIN`).
- [ ] Implementar las **tareas** (funcionalidades).
    - Crear los bloques funcionales de cada tarea una por una (`FB_*_SFC`), incluyendo `FB_EstacionPreparar_SFC`, `FB_EstacionFinalizar_SFC`, `FB_EstacionRestaurar_SFC`.
    - Dotar a cada bloque funcional de tarea de la estructura de rutina SFC (`Execute`, `Ack`, `Ready`, `Done`) (`FB_*_SFC`).
    - Incluir en cada tarea un **parámetro de entrada** para cada sensor que necesite utilizar y un **parámetro de salida** para cada actuador que necesite para cumplir su función (`FB_*_SFC`).
    - Integrar cada tarea en la estación (declarar una variable cuyo tipo es el correspondiente **FB**, invocarlo en el código y especificar sus parámetros de entrada y salida) (`FB_Estacion`).
    - Validar cada tarea antes de iniciar la siguiente, usando el pulsador de marcha para iniciar la tarea seleccionada (`FB_Estacion`, `FB_*_SFC`).
- [ ] Crear un **coordinador** de tareas secuencial en lenguaje `SFC` (`FB_EstacionCoordinador_SFC`).
    - Dotar al bloque funcional de coordinación de la estructura de rutina SFC.
    - Implementar únicamente la secuencia principal «directa» (la equivalente a un ciclo completo de producción).
    - El coordinador dispondrá de parámetros de entrada y salida para comunicarse con cada tarea de forma que, al menos, tenga un parámetro de salida (indicado en imperativo) para dar la orden de ejecución de la funcionalidad y un parámetro de entrada (indicado en participio pasado) para recibir la respuesta de que la tarea se ha completado satisfactoriamente. Ejemplo: `AlimentaBase` y `BaseAlimentada`.
    - Integrar el coordinador de tareas en la estación (declarar una variable de tipo `FB_EstacionCoordinador_SFC`, invocarlo y especificar sus parámetros de entrada y salida) (`FB_Estacion`).
    - Integrar el coordinador con cada una de las tareas (ajustar los parámetros de entrada/salida) en el `FB_Estacion`.
    - Usar el pulsador de marcha para iniciar una **prueba** de ejecución del coordinador (`FB_Estacion`, `FB_EstacionCoordinador_SFC`).
- [ ] Crear un **director** para la gestión del modo de funcionamiento en lenguaje `SFC` (`FB_EstacionDirector_SFC`).
    - Incluir la secuencia principal para la **Preparación**, **Producción** y **Finalización** (`FB_EstacionDirector_SFC`, `FB_*_SFC`).
- [ ] Incluir la gestión de las condiciones iniciales (`FB_Estacion`).
- [ ] Incluir la gestión de la tarea: contadores de maniobras (`FB_Estacion`, `FB_EstacionDirector_SFC`, `FB_EstacionCoordinador_SFC`).
    - Declaración en `FB_Estacion`.
    - Paso como parámetros de entrada a `FB_EstacionDirector_SFC` y `FB_EstacionCoordinador_SFC`.
    - Inicialización en `FB_EstacionDirector_SFC`.
    - Actualización en `FB_EstacionCoordinador_SFC`.
- [ ] Incluir la gestión de la falta de material en la tarea que se encargue de ella (`FB_*_SFC`).
- [ ] Incluir la gestión del tipo de pieza, si procede, en la tarea que se encargue de ella y en la visualización (`FB_*_SFC`, `VISU_Estacion`).
- [ ] Incluir la gestión del panel de operador (señalización) (`FB_Estacion`).
- [ ] Implementar la parada solicitada a final de ciclo (`FB_EstacionDirector_SFC`).
- [ ] Implementar la parada inmediata (`SFCPause`) con detención de los actuadores (según convenga) (`FB_Estacion`).
- [ ] Incluir funcionalidades adicionales.
    - Funcionamiento ciclo-a-ciclo (`FB_Estacion`).
    - Reinicio (`SFCReset`).
    - Incluir la secuencia de **Restauración** (`FB_EstacionDirector_SFC`, `FB_*_SFC`).
    - Secuencias paralelas (**opcional**) (`FB_EstacionCoordinador_SFC`).
