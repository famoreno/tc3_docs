# ✔ Conceptos generales

??? info "Leyenda"
    | Abrev. | Significado |
    |-------|-------------|
    | **CD** | Clic derecho del ratón |
    | **CI** | Clic izquierdo del ratón |
    | **DCI** | Doble clic izquierdo del ratón |
    | **TC3** | TwinCAT3 |
    | **CV** | Campus Virtual |
    | **FB** | Bloque funcional (*Functional Block*) |

---
## 🏗️ Crear soluciones en TC3

### Crear proyecto TC3

1. Abrir el *software* `Twincat XAE Shell`, desde el menú **Inicio** de Windows o desde el icono de la barra de programas en segundo plano que hay abajo a la derecha en la barra de tareas.
2. Seleccionar ***New TwinCAT Project***.

    ![Imagen](images/cb/image.png){width=240px}

3. Seleccionar el tipo ***TwinCAT XAE Project (XML format)***.

    ![Imagen](images/cb/image%201.png){width=384px}

4. Darle un nombre a la **Solución**, y seleccionar su ubicación (la que viene por defecto está bien). Dejar marcada la opción ***Create directory for solution***.

    ![Imagen](images/cb/image%202.png){width=768px}

    !!! question "Ejemplo"
        `TC3_Lampara`  

5. Por defecto, tanto la **Solución** de Visual Studio como el proyecto de **TC3** tendrán el mismo nombre.

!!! note "Recomendación"
    Ocultar las secciones del proyecto que no se van a utilizar: `MOTION`, `SAFETY`, `C++`, `VISION`, `ANALYTICS`.
    Nos quedaremos solo con `SYSTEM`, `PLC` e `I/O`.

### Crear proyecto PLC

1. Una vez creado un proyecto de TC3, procedemos a crear un proyecto PLC.
2. Hacer **CD** sobre la sección `PLC` y seleccionar ***Add New Item***.
3. Seleccionar ***Standard PLC Project***, darle un nombre y pulsar ***Add***.  
    ![Imagen](images/cb/image%203.png){width=384px}
        
    !!! question "Ejemplo"
        `Lampara_PLC`

1. En la sección de `SYSTEM > Tasks` aparecerá por defecto una nueva tarea `PLC Task` con sus parámetros por defecto (ej. 10 ms de ciclo).
2. En la sección `PLC` aparece el proyecto con dos secciones nuevas:
    1. `Project`
          1. `External Types`. Almacena definiciones de tipos de datos externos que provienen de fuentes externas al PLC.
          2. `References`. Listado de referencias a las librerías utilizadas en el proyecto.
          3. `DUTs`. Tipos de Dato de Usuario (*Data User Types*) (`ENUM`, `STRUCT`).
          4. `GVLs`. Listas de Variables Globales (*Global Variables Lists*).
          5. `POUs`. Unidades de Organización del Programa (Program Organization Units). Programas, bloques funcionales y funciones que implementaremos.
          6. `VISUs`. Visualizaciones creadas.
          7. Tarea creada (`PLCTask`) y programa `MAIN`
   
            ![Imagen](images/cb/image%204.png){width=132px}
    
    2. `Instance`. Aquí aparecerán las variables en las imágenes de Entrada y Salida.
 
3. A partir de aquí se puede empezar a implementar el proyecto.

### Crear bloque funcional

1. Hacer **CD** sobre la sección `POUs`.
2. Seleccionar `Add → POU → Functional Block`.
3. Darle un nombre significativo.
4. Seleccionar el lenguaje a utilizar. Normalmente utilizaremos `ST` o `SFC`.

---

## 🏷️ Declaración de variables

!!! tip "Recomendación"
    Se recomienda utilizar la convención **[CamelCase](https://es.wikipedia.org/wiki/Camel_case)** para declarar las variables.

    Como convención adicional, añadiremos un prefijo `i_` para aquellas variables que se declaren en la zona de entrada y `o_` para las de la zona de salida.

- Independientemente del lenguaje utilizado para implementar el código, las variables se declaran de la misma manera.
- Las variables se declaran en la caja superior de la ventana del bloque funcional creado.
- La sintaxis para la declaración de variables es la siguiente:

```pascal
<NombreVariable> : <tipo> [:=<ValorInicial>]
```

!!! question "Ejemplo"
    ```pascal
    // bool
    Pulsador: BOOL;
    LuzAmarilla: BOOL := TRUE;

    // enteros con y sin signo
    Altura: INT;
    Contador: UINT;
    UnidadesSolicitadas: UINT := 10;

    // números reales
    TpoSegundos: FLOAT := 1.2;

    // tiempo
    TiempoEspera: TIME := T#2s;
    TiempoRestante: TIME;

    // bloques funcionales
    Flanco_Pulsador: R_TRIG; // detector de flanco (estándar)
    Coordinador: FB_Coordinador; // bloque funcional definido por el usuario

    // arrays
    Ocupado: ARRAY[0..3] OF BOOL; // array de cuatro elementos de tipo BOOL; acceso con []
    ```

- La sintaxis de los valores posibles es la siguiente:
    - **Variables booleanas:** `TRUE`, `FALSE`.
    - **Variables enteras:** `0`, `1`, etc.
    - **Variables reales:** `0.1`, `2.3`, etc.
    - **Variables de tiempo:** `T#<tiempo>` donde `<tiempo>` debe ser del estilo `<numero><unidad>`, siendo `<unidad>` escogido de `{s, ms}`.

        !!! question "Ejemplo"
            `T#2s` (dos segundos)
            
            `T#500ms` (quinientos milisegundos).

    - El acceso a los *arrays* se hace con el índice entre corchetes. 
        
        !!! question "Ejemplo"
            `Ocupado[1] := TRUE;`

- Las variables en TC3 se declaran dentro de los ámbitos existentes en el POU correspondiente: **marcas** (locales), **entrada** y **salida**.

### Variables locales

```pascal
FUNCTIONAL_BLOCK FB_Estacion
VAR
    Contador: UINT;
END_VAR
```

Las variables declaradas aquí se pueden utilizar dentro del POU pero **no** pueden tomar valores de fuera del POU ni se pueden acceder desde fuera del POU.

### Variables de entrada

```pascal
FUNCTIONAL_BLOCK FB_Estacion
VAR_INPUT
    TiempoEntrada: TIME;
    TiempoSalida: TIME := T#2s;
END_VAR
```

Las variables declaradas aquí **deben** ser especificadas al llamar al **FB** (a no ser que se les de un valor por defecto).

!!! tip "Importante"
    No especificarlas en la llamada produce un error de compilación.

### Variables de salida

```pascal
FUNCTIONAL_BLOCK FB_Estacion
VAR_OUTPUT
    LuzAmarilla: BOOL;
    LuzVerde: BOOL;
END_VAR
```

Las variables declaradas aquí pueden ser accedidas desde fuera del **FB** (por ejemplo, desde otro **FB** que llama a este, o desde el programa `MAIN`).

```pascal
PROGRAM MAIN
VAR
    Estacion: FB_Estacion
    LuzAmarillaEstacion: BOOL;
END_VAR
------------------
LuzAmarillaEstacion := Estacion.LuzAmarilla; // esto es válido
```

!!! tip "Importante"
    Querer acceder a una variable de un **FB** que no ha sido declarada como salida produce un error de compilación.

### Variables de entrada y salida

```pascal
FUNCTIONAL_BLOCK FB_Estacion
VAR_IN_OUT
    Contador: UINT;
END_VAR
```

Este ámbito no aparece por defecto al crear un **FB** pero puede ser añadido simplemente escribiendo la sección `VAR_IN_OUT ... END_VAR`.

Las variables declaradas aquí deben tomar un valor como entrada al **FB** y su valor final tras cada ciclo puede ser accedido desde fuera del **FB**.

Combina las condiciones de los ámbitos de entrada y salida.

- En un programa (ej. `MAIN`) sólo disponemos del ámbito `VAR`.
- En un **FB**, además del ámbito `VAR`, disponemos de los ámbitos `VAR_INPUT` y `VAR_OUTPUT`.

---

### Variables vinculadas a la E/S

Recuerda que la memoria del PLC está estructurada en tres secciones: la **imagen de entrada**, la **imagen de salida** y la **zona de marcas**.

#### Zona de marcas

Aquí se guardarán aquellas variables que son internas al programa y no van a ser vinculadas con terminales de entrada y salida.

Las variables declaradas con la siguiente sintaxis se guardan en la zona de marcas:

```pascal
VAR
    Pulsador: BOOL;
    Contador: UINT;
    Lampara: BOOL;
END_VAR
```

#### Imagen de entrada

Aquí se guardarán las variables que queremos vincular a las **entradas físicas** del sistema.

Su sintaxis añade `AT %I*` antes de la definición del tipo:

```pascal
VAR
    i_Pulsador AT %I*: BOOL;
END_VAR
```

!!! warning "Importante"
    Recuerda que en nuestro trabajo usaremos la convención de añadir un prefijo `i_` delante de estas variables.

#### Imagen de salida

Aquí se guardarán las variables que queremos vincular a las **salidas físicas** del sistema.

Su sintaxis añade `AT %Q*` antes de la definición del tipo:

```pascal
VAR
    o_Lampara AT %Q*: BOOL;
END_VAR
```

!!! warning "Importante"
    Recuerda que en nuestro trabajo usaremos la convención de añadir un prefijo `o_` delante de estas variables.

!!! warning "Importante"
    Declarar una variable en la imagen de entrada o salida es **independiente** de que sean entradas o salidas del bloque funcional.
    
    Estas declaraciones son completamente correctas.

    ```pascal
    VAR_INPUT
        Pulsador AT %I*: BOOL;
        LamparaAmarilla AT %Q*: BOOL;
        TiempoEspera: TIME;
    END_VAR

    VAR
        LamparaMarcha AT %Q*: BOOL;
    END_VAR

    VAR_OUTPUT
        PresenciaPale AT %I*: BOOL;
    END_VAR
    ```

## 👫 Creación de DUTs

Además de los tipos de datos simples (`BOOL`, `INT`, etc.) y los bloques funcionales ya existentes (`R_TRIG`) o creados por nosotros (`FB_Coordinador_ST`), en ocasiones podemos necesitar crear tipos de dato que están compuestos por otros tipos de dato. A estos tipos de datos se les denomina **tipos de unidad de datos** (*Data Unit Type*, DUT).

En nuestros proyectos vamos a poder hacer uso de dos de ellos: 

- **Estructuras** (`STRUCT`)
- **Enumeraciones** (`ENUM`).

### Estructuras

Aglutinan en su interior otros tipos de dato y se definen haciendo **CD** sobre la carpeta DUTs y escogiendo `Add → DUT`:

![Imagen](images/cb/image%2016.png){width=240px}

Se le da un nombre significativo (se recomienda comenzar con `ST` como, por ejemplo, `ST_PIEZA`), se deja marcado ***Structure*** y se pulsa en ***Open***.

Se abrirá una ventana de texto para definir los componentes del tipo de dato:

```pascal
TYPE ST_PIEZA :
STRUCT
    Blanca: BOOL;
    Baja: BOOL;
    Tamano: INT;
    Tiempo: TIME;
END_STRUCT
END_TYPE
```

Para usarlo, se define una variable de ese tipo y se accede a sus componentes mediante el operador `.` (punto):

```pascal
// Definicion de variables
VAR
    Pieza: ST_PIEZA;
    Baja: BOOL;
END_VAR
--------------------------------
// Implementacion
Pieza.Blanca := TRUE;
Baja := (Pieza.Tamano < 12);
Pieza.Tiempo := T#1s;
```

### Enumeraciones

Una enumeración (`ENUM`) es un tipo de datos definido por el usuario compuesto por una serie de componentes separados por comas, también llamados valores de enumeración, que se utiliza para declarar variables definidas por el usuario.

Se definen haciendo **CD** sobre la carpeta DUTs y escogiendo `Add → DUT`:

![Imagen](images/cb/image%2016.png){width=240px}

Se le da un nombre significativo (se recomienda comenzar con `E` como, por ejemplo,  `E_ColorBasic`), se deja marcado ***Enumeration*** y se pulsa en ***Open***.

Se abrirá una ventana de texto para definir los componentes del tipo de dato:

```pascal
{attribute 'qualified_only'}
{attribute 'strict'}
TYPE E_ColorBasic :
(
    eRed, 
    eYellow,
    eGreen,
    eBlue,
    eBlack
) // Basic data type is INT, default initialization is eRed
;
END_TYPE
```

Una variable definida con el tipo `E_ColorBasic` es, en realidad, de tipo `INT` y solo puede tomar los valores definidos en `E_ColorBasic`: `eRed`, `eYellow`, etc.

Cada uno de esos valores tiene un valor `INT` asociado, comenzando por el cero: `eRed = 0`, `eYellow = 1`, etc.

Para usarlo, se define una variable de ese tipo y se le asigna el valor deseado usando el nombre del tipo de dato ***Enumeration***:

```pascal
// Definicion de variables
VAR
    Color: E_ColorBasic;
END_VAR
--------------------------------
// Implementacion
Color := E_ColorBasic.eYellow; // asignacion de valor
IF Color <> E_ColorBasic.eRed THEN // comprobacion de valor
    [...]
END_IF
```

## 📄 Lenguaje ST

!!! tip "Recomendación"
    Es recomendable acceder a la ayuda y documentación del lenguaje ST (*Structured Text*) que ofrece Beckhoff en su portal **[Infosys](https://infosys.beckhoff.com/)**.
  
### Sintaxis general
- Las **instrucciones** deben terminar con `;`.
- Los **comentarios** se pueden realizar con `//` hasta final de línea o metiendo el texto entre `(*` y `*)`.
- La **asignación** de valores entre variables se realiza con el operador `:=`.
- La **comparación** de valores se realiza con los operadores `=`, `<>`, `<=`, `>=`.
- Las **operaciones lógicas** se realizan con los operadores `AND`, `OR` y `NOT`.
- La llamada a los **FBs** se realiza escribiendo el nombre de la instancia del **FB** seguido de, entre paréntesis, las asignaciones de los valores para las variables de entrada (si las hay), separadas por comas: `<nombre_instancia>(var1:=val1, var2:=val2, ...);`
- En caso de que no haya ninguna variable de entrada que especificar, simplemente se abre y se cierra paréntesis.

    !!! question "Ejemplo"
        `Estacion();`

        `Lampara(TiempoEncedido:=T#2s);`

### Estructuras de control
- Las **estructuras de control** básicas son:
    - Condicionales (`if`, `case`)
        ```pascal
        IF <condition> THEN
            <statements>
        ELSIF <condition> THEN
            <statements>
        ELSE
            <statements>
        END_IF;

        CASE <expression> OF
            <value>, <value>, …, <value>: <statements>
        ELSE
            <statements>
        END_CASE;
        ```
    - Bucles (`for`, `while`, `repeat`)
        ```pascal
        FOR <variable> := <expression> TO <expression> BY <expression> DO
            <statements>
        END_FOR;

        WHILE <condition> DO
            <statements>
        END_WHILE;

        REPEAT
            <statement>
        UNTIL <condition>
        END_REPEAT;
        ```

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
      
    ![Imagen](images/cb/image%205.png){width=288px}

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
         
            ![Imagen](images/cb/image%206.png){width=384px}

        - En nuestros proyectos, **estas acciones siempre serán en `ST`**, pero podrían ser implementadas en cualquier otro lenguaje de la norma.
        - Una vez creada, aparece en el programa `SFC` como un cuadrado con una **E** en la esquina inferior izquierda de la etapa.
         
            ![Imagen](images/cb/image%207.png){width=288px}

    - **A la salida**
        - Las acciones con activación **a la salida** se ejecutan solo una vez inmediatamente antes de pasar a la siguiente etapa. Esto implica que **antes** de que se ejecute esta acción, la condición de transición para pasar a la siguiente etapa **debe ser cierta**.
        - Normalmente usaremos estas acciones para inicializar variables memorizadas, actualizar contadores, etc.
        - Para crear una de este tipo, hacer **CD** sobre la etapa donde la queremos asociar y seleccionar ***Add exit action***.
        - Aparece un *popup* donde se nos pregunta por el nombre que le queremos poner y el lenguaje a utilizar. Se recomienda dejar el nombre por defecto (`S0_exit` en la figura) ya que nos indica en qué etapa está y de qué tipo es.

            ![Imagen](images/cb/image%208.png){width=384px}

        - En nuestros proyectos, **estas acciones siempre serán en `ST`**, pero podrían ser implementadas en cualquier otro lenguaje de la norma.
        - Una vez creada, aparece en el programa `SFC` como un cuadrado con una **X** en la esquina inferior derecha de la etapa.         
            ![Imagen](images/cb/image%209.png){width=288px}

            !!! warning "Importante" 
                Nada impide que una etapa tenga asociadas **una o varias acciones no memorizadas**, una con activación a la entrada y otra con activación a la salida.
        
#### Acción principal (TODO)

---
## 🔀 Estructuras de evolución

### Secuencia básica

- Una secuencia básica se compone de una **sucesión lineal de etapas y transiciones**, donde las primeras se van a ir ejecutando en secuencia conforme las condiciones asociadas a las segundas se vayan cumpliendo.

- Normalmente, al final de la secuencia se producirá un salto hacia atrás (o el inicio) en el programa.

    ![Imagen](images/cb/image%2010.png){width=288px}

- Para insertar un salto detrás de una transición, hay que hacer **CD** sobre la transición y seleccionar ***Insert jump after***. Solo hay que indicar el nombre de la etapa a la que queremos saltar.

### Bifurcación

- Tras una etapa podemos realizar una **bifurcación** en distintas ramas en función de distintas condiciones. Esto nos permite dirigir la secuencia por cambio si ocurre un evento y por otros distintos si ocurren otros eventos.

- En el ejemplo de la figura, si la etapa `Init` está activa y se activa `Execute`, el programa evolucionará por la rama de la izquierda llegando a `S0`. Si lo que se activa es `Restore`, el programa evolucionará por la derecha pasando a `Sr` y, una vez se active `Restaurado`, la secuencia pasará a `S0`.

    ![Imagen](images/cb/image%2011.png){width=336px}

- Para realizar una bifurcación, hacer **CD** sobre la **transición** donde se quiera hacer la bifurcación (`Execute` en el ejemplo) y seleccionar ***Insert branch right***.

- Nada impide que se pueda hacer una bifurcación con más de dos ramas.

- Es recomendable que las condiciones de la bifurcación sean excluyentes pero nada impide que no lo sean. El programa tomará el camino de la primera transición cuya condición sea verdadera.

- Si ocurriera que varias o todas las condiciones son verdaderas a la vez, el programa evolucionará por la rama de la izquierda. **Aunque esto puede ser útil en algunos casos, esto suele indicar que hay un mal diseño en el programa.**

### Paralelismo

- Si queremos que el programa evolucione por dos secuencias en paralelo (se ejecutan simultáneamente) podemos incluir un paralelismo en el código.

- En el ejemplo de la figura, si la etapa `Init` está activa y se activa `Execute`, el programa evolucionará por ambas ramas a la vez, activando los estados `S0` y `Sr` de manera simultánea (y por tanto, `LuzRoja` y `Restaura`).

    ![Imagen](images/cb/image%2012.png){width=480px}

- En la transición con condición `NOT Pulsador OR S0.t>T#5s` se produce un punto de sincronización ya que, para que el programa evolucione a `S1` debe ocurrir que `S0` y `Sr2` estén activas y, además, que la condición `NOT Pulsador OR S0.t>T#5s` sea cierta. Por tanto, podemos decir que el programa *esperará* hasta que termine la rama de la derecha antes de evolucionar.

---

## 🖥️ Crear visualización

- Hacer **CD** sobre la sección `VISUs`.

- Seleccionar `Add → Visualization` y pulsar en **Open** en la ventana *popup*.

    ![Imagen](images/cb/image%2013.png){width=240px}

- En la parte derecha de la pantalla aparecerá la sección `Toolbox` donde, en la sección `Basic` aparecen las formas básicas. Arrastrar a la visualización los elementos que se quieran.

!!! warning "Importante"
    Si no aparece la sección, mostrarlo entrando en el **Menú** `View → Toolbox`

!!! note "Recomendación"
    Se recomienda utilizar **rectángulos** para crear botones tanto para las entradas como para las salidas.

### Botones para cambiar valores de variables

- Dibujar un rectángulo con el tamaño deseado.

- Escribir dentro la etiqueta que queramos que aparezca en el botón.

- Introducir la variable de tipo `BOOL` que queremos asociar a dicho botón. Dependiendo del comportamiento que queramos que tenga el botón, esta variable se introduce en una sección distinta dentro de `Properties → Input Configuration` (la pestaña `Properties` aparece a la derecha, normalmente combinada con `Toolbox`).

    - Si queremos que la variable cambie de valor **mientras** se pulsa el botón con el ratón pero vuelva a su valor anterior una vez soltado el ratón, introduciremos la variable en la sección `Tap`:

        ![Imagen](images/cb/image%2014.png){width=240px}

    - Si queremos que la variable cambie de valor cada vez que pulsemos el botón (el valor conmutará entre `TRUE` y `FALSE`) lo introduciremos en la sección `Toggle`:

        ![Imagen](images/cb/image%2015.png){width=240px}


---

## 🔄 Exportar e importar

1. Podemos exportar `POUs` y `VISUs` desde una solución de **TC3** e importarla de nuevo en otra distinta. De esta manera podemos reutilizar código de distintas proyectos.
2. Para realizar esto, en la solución origen, simplemente hay que hacer **CD** sobre el `POU` o `VISU` a exportar y seleccionar ***Export to ZIP***. Se selecciona donde guardar el archivo exportado y se pulsa **Save**.
3. Posteriormente, en la solución destino, hacer **CD** sobre la carpeta `POU` o `VISU` y seleccionar ***Import from ZIP***. Se busca el archivo correspondiente y se pulsa **OK**.

---

## 💾 Guardar y mover proyectos

!!! warning "Importante"
    Se ha detectado que el sincronizar la carpeta del proyecto usando servicios como **Google Drive** está produciendo problemas a la hora de poder abrir los proyectos. Posiblemente esto se deba a que algunos ficheros no son sincronizados correctamente por Drive (por motivos desconocidos), lo que lleva a que, a la hora de abrir el proyecto, no se carguen los ficheros necesarios. 
    
    **Se recomienda, por tanto, no usar este método sino alguno de los otros.**

### Usando la carpeta completa

1. Es la manera más sencilla de llevarse un proyecto desde un equipo a otro.
2. Solo hay que copiar la carpeta raíz en un *pendrive* y pegar la carpeta en el equipo destino.

    ![Imagen](images/cb/image%2017.png){width=132px}

4. Posteriormente, hacer **DCI** sobre el fichero de ***Solution*** (`.sln`) para que se abra de nuevo en TC3.

!!! warning "Importante"
    Si la carpeta ha sido comprimida para ser trasladada, hay que asegurarse de haber descomprimido la carpeta completa en el destino antes de abrir el proyecto.

### Exportando como ```.tnzip```

!!! warning "Importante"
    La entrega final del proyecto deberá seguir este procedimiento.

1. Este proceso genera el mínimo tamaño posible para trasladar un proyecto.
2. Seleccionar `File → Save [nombre_del_proyecto] as Archive...`.
3. Seleccionar dónde guardar el proyecto, darle un nombre y asegurarse de que el formato es de tipo `.tnzip`.
4. Para volver a abrir el proyecto:
   - Abrir TC3.
   - Seleccionar `File → Open → Solution from Archive...`.
   - Buscar el archivo `.tnzip`.
   - Seleccionar (o crear si no existe) una carpeta donde se va a generar la *Solution*.
   - En principio, se puede seleccionar siempre la misma carpeta cada vez que se repita este procedimiento.

!!! warning "Importante"
    Si al abrir el proyecto de nuevo y compilar obtienes errores no relacionados con el código que antes no tenías:

    - Cambiar el tipo de proyecto a TwinCAT RT (x86)
    - Recompilar
    - Volver a cambiar a TwinCAT RT (x64)
    - Volver a recompilar

### Usando GIT

TwinCAT3, al estar basado en Visual Studio, tiene compatibilidad directa con GitHub. 
Se recomienda seguir el tutorial en este video:

[PLC Programming using TwinCAT 3 - Version control](https://www.youtube.com/watch?v=1g6eYnlzKtA)

---

## 📝 Convenciones de nombres

Se recomienda llamar a todos los elementos del proyecto con el nombre adecuado **desde el principio**, ya que renombrar *a posteriori* puede acarrear problemas derivados del acceso a elementos cuya ruta ha cambiado. No obstante, si es necesario renombrar los elementos, el procedimiento es como sigue.

### Soluciones

Se recomienda llamar a las soluciones de TC3 de la misma forma que los Proyectos de TC3.

Para renombrarlo una vez creado:

  1. **CD** sobre el nombre de la solución y seleccionar ***Rename***.

    ![Imagen](images/cb/image%2018.png){width=240px}

  2. Escribir el nuevo nombre.

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

            ![Imagen](images/cb/image%2019.png){width=240px}

        - Escribir el nuevo nombre.

### Proyectos PLC

- Se recomienda llamar a los proyectos PLC de la forma `[nombre]_PLC` para los ejemplos o `[estacion]_[nivel]_PLC` para los trabajos, donde el nombre debe ser algo significativo.

    !!! question "Ejemplo"
        `Lampara_PLC`
            
        `FMS201_Monolitico_PLC`

- Para renombrarlo una vez creado:
    1. **CD** sobre el nombre del proyecto PLC y seleccionar *Rename*.

        ![Imagen](images/cb/image%2020.png){width=240px}

    3. Escribir el nuevo nombre.
    4. Aparecerá un aviso indicando que si se cambia el nombre del proyecto no se van a poder hacer cambios online (en caso de que se esté ejecutando). **CI** en Sí.

        ![Imagen](images/cb/image%2021.png){width=432px}

    5. Tras unos segundos, el proyecto PLC habrá cambiado de nombre.
    6. **Importante:** Puede ocurrir que, tras el cambio de nombre, al hacer **CI** sobre el proyecto, salga un aviso de error por no encontrar el nombre anterior. Esto se debe solucionar haciendo **CI** sobre `Build → Rebuild Solution`.

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
    
        ![Imagen](images/cb/image%2022.png){width=384px}
    
    4. Aparecerá una ventana mostrando todos los cambios que se van a realizar. Pulsar en **OK**.
    
        ![Imagen](images/cb/image%2023.png){width=598px}

### Variables

- Se recomienda llamar a las variables con nombres significativos.
- Si las variables se van a asociar con los terminales de entrada y salida, es **obligatorio** llamarlas con el nombre indicado en la columna **Variable** de la tabla de E/S.
- Para renombrar una variable y que ese cambio se corrija en todas las referencias que se hagan a la misma en el proyecto, hay que hacer **CD** sobre el nombre de la variable y seleccionar `Refactoring → Rename`.
- Aparecerá un *popup* donde se debe indicar el nuevo nombre.
    
    ![Imagen](images/cb/image%2024.png){width=240px}

- Aparecerá una ventana mostrando todos los cambios que se van a realizar. Pulsar en **OK**.

    ![Imagen](images/cb/image%2025.png){width=626px}

---

## ▶️ Ejecutar programa

Una vez el programa está implementado (independientemente del lenguaje utilizado):

- Compilar el proyecto: Menú `Build → Build [nombre del proyecto]`.
- Asegurarse de que no hay errores.
- Si has declarado variables en las **imágenes de entrada y/o salida**:     
    - Comprobar que las variables aparecen en la zona de la instancia.
        
        ![Imagen](images/cb/image%2026.png){width=200px}

- Si quieres utilizar el **Equipo Remoto**:
    - Vincular las variables con los terminales correspondientes según la tabla de E/S:
        - **DCI** sobre la variable a vincular.
        - Seleccionar el terminal/canal deseado del listado que aparece.
    - Buscar el equipo remoto.

- Si quieres usar el **Runtime local**
    - Asegurarte de que seleccionar ``Local` en el desplegable del `Target`. 
        
        ![Imagen](images/cb/image%2027.png){width=200px}

- Activar la configuración en el `Target` (***Activate Configuration***) y activar el modo de ejecución (***Run Mode***). Esto último te lo pregunta TwinCAT3 en una ventana *popup*.
        
    ![Imagen](images/cb/image%2028.png){width=50px}

- Descargar el programa en el `Target` (***Login***), donde se preguntará, en un *popup*, si quieres crear un puerto de comunicación con el `Target` y descargar el programa. Pulsar en ***Yes***.

    ![Imagen](images/cb/image%2029.png){width=400px}

- Ejecutar el programa (***Start***).

    ![Imagen](images/cb/image%2030.png){width=70px}

!!! warning "Importante"
    Para poder modificar de nuevo el programa, primero hay que parar el programa (***Stop***) [**recomendado**] y posteriormente hacer ***Logout***.

    ![Imagen](images/cb/image%2031.png){width=70px}
    

---

## 🔌 Activar/desactivar *hardware*

1. Si has vinculado las variables de tu programa con el **equipo remoto** (hiciste la búsqueda del equipo remoto y la exploración de los módulos de E/S), cuando quieras probar tu programa en el ***Runtime Local***, aparecerá una ventana *popup* indicando un error.
2. Esto se debe a que TC3 quiere establecer conexión con el *hardware* al que estuviste conectado pero no puede, ya que el `Target` es el local.
3. Para evitar esto, solo tienes que deshabilitar el *hardware* haciendo **CD** sobre el dispositivo buscado y seleccionar ***Disable***.

!!! warning "Importante"
    Recuerda **volver a habilitarlo** cuando quieras volver a usar el equipo remoto.

---

## 🌐 Búsqueda de equipos

!!! tip "Recomendación"
    Hay un video de ejemplo en el Campus Virtual en `Automatización > Videos > TC3` con nombre `9_Runtime_Target_*.mkv`.

---

## 🔗 Enlace de variables y E/S

!!! tip "Recomendación"
    Hay un video de ejemplo en el Campus Virtual en `Automatización > Videos > TC3` con nombre `9_Runtime_Target_*.mkv`.