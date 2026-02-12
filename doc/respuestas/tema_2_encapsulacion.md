<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación en Programación Orientada a Objetos busca agrupar en una misma unidad (la clase) los datos y las operaciones que trabajan sobre esos datos. La ocultación de información es el mecanismo que permite controlar qué partes internas de esa clase son accesibles desde fuera y cuáles no. Ambos conceptos están estrechamente relacionados y tienen como objetivo principal reducir el acoplamiento entre las distintas partes de un programa.

La ocultación de información pretende que el estado interno de un objeto no pueda ser modificado libremente desde el exterior. En lugar de acceder directamente a los atributos, el uso de métodos públicos obliga a interactuar con el objeto de una forma controlada. Esto permite que la clase mantenga su coherencia interna y haga cumplir ciertas reglas sobre sus datos.

Entre las ventajas de la ocultación de información se encuentran una mayor robustez del código, una reducción de errores, una mejora en la mantenibilidad y una mayor facilidad para modificar la implementación interna sin afectar al resto del programa. Además, facilita el razonamiento sobre el comportamiento de los objetos y el diseño modular del software.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de una clase es el conjunto de miembros (métodos y, en algunos casos, atributos) que están accesibles desde fuera de la clase. Define cómo otros objetos o clases pueden interactuar con ella, sin necesidad de conocer su funcionamiento interno. Es, en esencia, el “contrato” que la clase ofrece al exterior.

La ocultación de información se relaciona directamente con la interfaz pública porque todo aquello que no forma parte de dicha interfaz queda escondido. Los detalles de implementación, como estructuras internas o cálculos auxiliares, permanecen inaccesibles para el código externo.

Gracias a esta separación, una clase puede cambiar su implementación interna sin modificar su interfaz pública. Mientras el contrato se mantenga, el resto del programa no se ve afectado, lo que favorece la evolución y el mantenimiento del software.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública de una clase debe diseñarse con cuidado porque es la parte que será utilizada por otros componentes del programa. Una mala decisión en esta interfaz puede obligar a depender de detalles innecesarios o exponer funcionalidades que no deberían ser accesibles.

Cambiar la interfaz pública de una clase no suele ser sencillo, ya que cualquier modificación puede romper el código que la utiliza. Esto es especialmente problemático en proyectos grandes o cuando la clase forma parte de una biblioteca compartida.

Por este motivo, se recomienda que la interfaz pública sea mínima, clara y estable. Es preferible exponer sólo lo necesario y ocultar el resto, anticipando posibles cambios futuros en la implementación.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto de esa clase sea válido. Estas reglas describen estados correctos del objeto y deben mantenerse tras la ejecución de cualquier método público.

La ocultación de información ayuda a preservar las invariantes porque impide que el estado interno del objeto sea modificado directamente desde fuera. Al obligar a usar métodos controlados, la clase puede comprobar y garantizar que los cambios respetan dichas reglas.

De esta forma, se evita que el objeto entre en un estado inconsistente. Esto aumenta la fiabilidad del programa y simplifica el razonamiento sobre el comportamiento de los objetos.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
