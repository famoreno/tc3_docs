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

        `Lampara(TiempoEncedido := T#2s);`

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
