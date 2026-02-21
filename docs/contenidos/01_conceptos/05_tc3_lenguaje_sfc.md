## ⤵️ Lenguaje SFC

### Reglas sintácticas

- Los nombres de las etapas en SFC (*Sequential Function Chart*) no pueden empezar por un número. Tampoco pueden tener espacios, puntos u otros caracteres especiales como eñes, interrogaciones, etc. Sí permite guiones bajos.
- **No puede haber dos etapas consecutivas ni dos transiciones consecutivas**. Hay que tener especial atención a esto cuando se produzcan bifurcaciones o saltos.

### Añadir etapa / transición
- Hacer **CD** sobre la **etapa** donde queramos introducir una nueva y seleccionar ***Add step-transition*** o ***Add step-transition after***, dependiendo de si queremos añadirla antes o después, respectivamente, de la etapa seleccionada.

!!! warning "Importante"
    Comprobar que no quedan dos etapas o dos transiciones consecutivas. En caso contrario, borrar aquello que no sirva (**CI** sobre él y pulsar *Supr*).

### Asociar acciones a etapas
#### Acción continua
- Hacer **CD** sobre la **etapa** a la que queramos asociar una acción no memoriza (o continua) y seleccionar ***Insert action association*** o ***Insert action association after***, dependiendo de si queremos insertarla antes o después de las ya existentes (si las hay).
- En la caja de la acción aparece en primer lugar el **modificador** (por defecto `N`, que significa **"No memorizada"**) y en segundo lugar el hueco donde debemos poner la acción a realizar (una variable booleana que queramos activar o una acción más compleja definida como acción asociada al bloque funcional).
      
    ![Imagen](../images/01_conceptos/image%205.png){width=288px}

    - Tipos de modificadores de acciones

        | Código | Tipo | Descripción |
        |-------|------|-------------|
        | `N`  | **No memorizada (continua)** | **Se ejecuta/activa mientras la etapa esté activa.** |
        | `R0` | Reinicio | La acción se desactiva. |
        | `S0` | Activación | Se ejecuta cuando se activa la etapa y continúa activa aunque la etapa se desactive. |
        | `L`  | Limitada | Se ejecuta cuando se activa la etapa y se desactiva cuando la etapa se desactiva o se alcanza el tiempo especificado. |
        | `D`  | Retrasada | Se ejecuta un tiempo después de que se active la etapa y se desactiva cuando la etapa se desactiva. |
        | `P`  | Pulsada | Se ejecuta dos veces: cuando se activa la etapa y una vez más en el ciclo siguiente. |
        | `SD` | Activación con retardo | Se activa aunque la etapa ya no esté activa. |
        | `DS` | Retardo de activación | Se activa solo si la etapa permanece activa. |
        | `SL` | Activación limitada | Activación con duración limitada. |

        !!! warning "Importante"
            Usaremos, por defecto, las **acciones no memorizadas**, aunque se pueden usar las otras si tiene sentido para el proyecto.

#### Acción de entrada o salida
- Podemos crear acciones con activación **a la entrada** o **a la salida** de una etapa.
- Estas acciones se implementan en alguno de los lenguajes de la norma y permiten realizar acciones que se ejecutan **solo una vez** durante la etapa, en lugar de hacerse de manera continua.

    - **A la entrada**
        - Las acciones con activación a la **entrada** se ejecutan solo una vez **inmediatamente después** de entrar en la etapa donde se asocian. **Posteriormente** se comprueba si la condición de transición para pasar a la siguiente etapa es cierta o no.
        - Normalmente usaremos estas acciones para inicializar variables memorizadas, actualizar contadores, etc.
        - Para crear una de este tipo, hacer **CD** sobre la etapa donde la queremos asociar y seleccionar ***Add entry action***.
        - Aparece un popup donde se nos pregunta por el nombre que le queremos poner y el lenguaje a utilizar. Se recomienda dejar el nombre por defecto (`S0_entry` en la figura) ya que nos indica en qué etapa está y de qué tipo es.
         
            ![Imagen](../images/01_conceptos/image%206.png){width=384px}

        - En nuestros proyectos, **estas acciones siempre serán en `ST`**, pero podrían ser implementadas en cualquier otro lenguaje de la norma.
        - Una vez creada, aparece en el programa `SFC` como un cuadrado con una **E** en la esquina inferior izquierda de la etapa.
         
            ![Imagen](../images/01_conceptos/image%207.png){width=288px}

    - **A la salida**
        - Las acciones con activación **a la salida** se ejecutan solo una vez inmediatamente antes de pasar a la siguiente etapa. Esto implica que **antes** de que se ejecute esta acción, la condición de transición para pasar a la siguiente etapa **debe ser cierta**.
        - Normalmente usaremos estas acciones para inicializar variables memorizadas, actualizar contadores, etc.
        - Para crear una de este tipo, hacer **CD** sobre la etapa donde la queremos asociar y seleccionar ***Add exit action***.
        - Aparece un *popup* donde se nos pregunta por el nombre que le queremos poner y el lenguaje a utilizar. Se recomienda dejar el nombre por defecto (`S0_exit` en la figura) ya que nos indica en qué etapa está y de qué tipo es.

            ![Imagen](../images/01_conceptos/image%208.png){width=384px}

        - En nuestros proyectos, **estas acciones siempre serán en `ST`**, pero podrían ser implementadas en cualquier otro lenguaje de la norma.
        - Una vez creada, aparece en el programa `SFC` como un cuadrado con una **X** en la esquina inferior derecha de la etapa.         
            ![Imagen](../images/01_conceptos/image%209.png){width=288px}

            !!! warning "Importante" 
                Nada impide que una etapa tenga asociadas **una o varias acciones no memorizadas**, una con activación a la entrada y otra con activación a la salida.
        
#### Acción principal (TODO)
