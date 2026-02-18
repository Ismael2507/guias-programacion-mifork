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

java
    public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    }

La ocultación de información se consigue declarando los atributos x e y como private, de modo que no puedan ser accedidos directamente desde fuera de la clase. El cálculo de la distancia se realiza mediante un método público que utiliza esos datos internos.

La interfaz pública de la clase Punto está formada por su constructor y por el método calcularDistanciaAOrigen. Son los únicos elementos accesibles desde otras clases.

El modificador public indica que un miembro es accesible desde cualquier parte del programa, mientras que private restringe el acceso exclusivamente al interior de la clase.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de visibilidad como public y private pueden aplicarse a clases, atributos, métodos y constructores. Su uso determina desde dónde pueden ser accedidos dichos elementos.

El modificador public permite el acceso desde cualquier clase, independientemente del paquete en el que se encuentre. Por el contrario, private restringe completamente el acceso al interior de la propia clase.

Las clases de nivel superior no pueden ser private, pero sí los miembros que contienen. Esto refuerza la idea de encapsulación y control del acceso a los detalles internos.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En Programación Orientada a Objetos existen distintos niveles de visibilidad que controlan el acceso a los miembros de una clase. Los más habituales son público, privado y protegido, aunque algunos lenguajes incluyen niveles adicionales.

En Java existen cuatro niveles de visibilidad: public, protected, private y visibilidad por defecto (package-private). Esta última permite el acceso sólo desde clases del mismo paquete.

En otros lenguajes, como C++, también existe protected, pero no el concepto de paquete. Cada lenguaje adapta estos mecanismos a su propio modelo de organización del código.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros de instancia privados de un objeto están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto es un detalle importante del modelo de encapsulación en Java.

    public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
    }


Aunque x e y son privados, el método puede acceder a los atributos de otra instancia de Punto porque el acceso se realiza desde el interior de la misma clase.

Esto permite que los objetos cooperen entre sí manteniendo la ocultación frente al exterior.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter y setter son métodos públicos que permiten acceder y modificar atributos privados de una clase de forma controlada. Un getter devuelve el valor de un atributo, mientras que un setter permite cambiarlo.

Su uso evita el acceso directo a los atributos y permite introducir validaciones o lógica adicional al leer o modificar el estado del objeto. Esto refuerza la encapsulación.

No siempre es obligatorio definir getters y setters para todos los atributos. Su inclusión debe responder a una necesidad real de la interfaz pública.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se afirma que la ocultación de información mejora la “seguridad” del programa, no se hace referencia a la protección frente a ataques o hackeos. El concepto de seguridad aquí es interno al diseño del software.

Se trata de evitar usos incorrectos de los objetos, accesos indebidos a datos y estados inconsistentes. La ocultación protege al propio programa de errores de programación.

Por tanto, es una seguridad lógica y estructural, no una medida de ciberseguridad.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia pertenece a cada objeto concreto creado a partir de una clase. Cada instancia tiene su propia copia de esos datos. En cambio, un miembro de clase es compartido por todas las instancias.

En Java, los miembros de clase se declaran usando la palabra clave static. Estos miembros también pueden ocultarse usando modificadores de visibilidad como private.

Por tanto, la ocultación de información no se limita a los miembros de instancia, sino que también se aplica a los miembros de clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que un constructor sea privado en determinados diseños. Un constructor privado impide la creación directa de objetos desde fuera de la clase.

Esto se utiliza, por ejemplo, en patrones de diseño como el patrón factoría o el patrón singleton. En estos casos, la clase controla completamente cómo y cuándo se crean sus instancias.

De esta forma, se refuerza la encapsulación y el control del ciclo de vida de los objetos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase en Java se indican mediante la palabra clave static. Estos miembros pertenecen a la clase y no a una instancia concreta.

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;


Estos atributos pueden actualizarse en el constructor cada vez que se crea un nuevo punto, permitiendo conocer los valores máximos establecidos hasta el momento.

La ocultación se mantiene declarando estos miembros como private.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

    public static Punto crearPuntoRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
Sí, se ha usado static porque un método factoría pertenece a la clase y no a una instancia concreta. No necesita un objeto previo para ser invocado.

Este método encapsula la lógica de creación y evita que el código externo tenga que conocer los detalles del redondeo.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

    private double[] coords = new double[2];

La implementación interna cambia, pero la interfaz pública permanece igual. Los métodos públicos siguen ofreciendo el mismo comportamiento.

Esto demuestra una de las principales ventajas de la encapsulación: poder modificar la representación interna sin afectar al código cliente.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo tenga getter y setter públicos, no es recomendable declararlo público. El acceso directo elimina cualquier posibilidad de control futuro.

La convención habitual en POO es declarar los atributos como private. Esto permite añadir validaciones o cambiar el comportamiento sin romper la interfaz pública.

Esta práctica está directamente relacionada con el mantenimiento de las invariantes de clase.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase inmutable es aquella cuyos objetos no pueden cambiar su estado una vez creados. Cualquier operación que “modifique” el objeto devuelve uno nuevo.

Un método modificador es aquel que cambia el estado interno del objeto. Un setter es un tipo de método modificador, pero no todos los métodos modificadores son setters.

Las clases inmutables ofrecen ventajas como mayor seguridad, facilidad de razonamiento y ausencia de efectos colaterales.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir setters como norma general. Exponer setters innecesarios debilita la encapsulación y permite estados inconsistentes.

Los setters deben existir sólo cuando modificar el atributo desde fuera tiene sentido dentro del diseño de la clase.

Una buena interfaz pública es mínima y controlada.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Al concatenar dos cadenas, no se modifica ninguna de ellas, sino que se crea un nuevo objeto String.

Esto puede ser ineficiente cuando se realizan muchas concatenaciones sucesivas. En esos casos, se recomienda usar clases como StringBuilder.

Estas clases están diseñadas para construir cadenas de forma eficiente paso a paso.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO, los objetos pueden compararse por identidad o por contenido. La identidad indica si dos referencias apuntan al mismo objeto.

En Java, el método equals se utiliza para comparar el contenido lógico de los objetos. Por defecto, equals compara identidad, igual que ==.

Las cadenas en Java deben compararse siempre usando equals, no ==.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan tipos primitivos en objetos. En Java, ejemplos son Integer, Double o Boolean.

El proceso de conversión puede ser automático mediante autoboxing y unboxing. Esto facilita el uso de primitivas en colecciones y contextos orientados a objetos.

No todos los lenguajes orientados a objetos tienen tipos primitivos, por lo que no todos necesitan wrappers.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo enumerado es un conjunto finito de valores constantes con significado semántico. Representa un dominio cerrado de valores posibles.

En Java, un enumerado es una clase especial. Puede tener atributos, métodos y constructores privados.

Esto mejora la encapsulación al asociar comportamiento directamente a los valores posibles.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        return esHemisferioNorte ? ordinal >= 3 && ordinal <= 5 : ordinal >= 9 && ordinal <= 11;
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        return esHemisferioNorte ? ordinal >= 6 && ordinal <= 8 : ordinal == 12 || ordinal <= 2;
    }

    public boolean esDeOtono(boolean esHemisferioNorte) {
        return esHemisferioNorte ? ordinal >= 9 && ordinal <= 11 : ordinal >= 3 && ordinal <= 5;
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        return esHemisferioNorte ? ordinal == 12 || ordinal <= 2 : ordinal >= 6 && ordinal <= 8;
    }
}
Este enumerado encapsula datos y comportamiento, usando atributos privados y constructores controlados. Representa un uso completo de encapsulación en Java.


