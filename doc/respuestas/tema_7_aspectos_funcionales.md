<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función es una variable que almacena la dirección de memoria donde comienza el código ejecutable de una función. A diferencia de los punteros de datos, que apuntan a variables en la pila o el montón, estos permiten tratar a las funciones como entidades que pueden ser referenciadas y pasadas dinámicamente. Esto facilita la implementación de algoritmos genéricos y sistemas de "callbacks" en lenguajes como C.

En la arquitectura de memoria, cuando se compila un programa, cada función ocupa un bloque de instrucciones. El nombre de la función actúa, en esencia, como una constante que representa esa dirección inicial. Al declarar un puntero de este tipo, se debe especificar la firma exacta de la función (tipo de retorno y tipos de parámetros) para que el compilador sepa cómo gestionar la pila de llamadas al invocarla.

````C
#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "hola mundo";
    // Declaración del puntero a la función
    char* (*aMayusculas)(char*);
    
    // Asignación de la dirección de la función al puntero
    aMayusculas = &convertirMayusculas;
    
    // Invocación a través del puntero
    printf("%s\n", aMayusculas(texto));
    
    return 0;
}
````

/*
Las funciones son ciudadanos de primera clase. Una funcion es nuun tipo mas:
- Se puede asignar a una variable
- Pasar como parametro
- Devolver una funcion como retorno de otra

Closure
Expresiones Lambda
En lenguajes con comprobacion estatica de tipo: Que tipo tienen?
*/

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima, es decir, una función que se define sin un nombre identificativo asociado. Se utiliza principalmente para representar bloques de código que se ejecutan de manera inmediata o que se pasan como argumentos a otras funciones. Su principal ventaja es la concisión, permitiendo escribir lógica directamente donde se necesita sin necesidad de declarar una función formal en el ámbito global o de clase.

Estas funciones son fundamentales en la programación moderna porque permiten tratar el comportamiento como si fuera un dato. En lugar de definir una estructura compleja de clases para una operación simple, la lambda encapsula la lógica en una expresión compacta. En lenguajes de tipado estático como Java, estas expresiones deben coincidir con una interfaz específica, mientras que en lenguajes dinámicos como JavaScript, son objetos de primera clase.

Ejemplo en JavaScript:
````JavaScript
const aMayusculas = (cadena) => cadena.toUpperCase();
console.log(aMayusculas("hola mundo"));
````
Ejemplo en Java:

````Java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
````

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación centrado en el uso de funciones matemáticas puras y en evitar el cambio de estado y los datos mutables. En este modelo, el cómputo se trata como la evaluación de expresiones en lugar de una ejecución de comandos secuenciales que modifican la memoria del programa. Se prioriza la declaración de "qué" se quiere obtener sobre el "cómo" se debe procesar paso a paso.

Se denomina a lenguajes como Java 8 multi-paradigma porque, aunque mantienen su estructura fundamental orientada a objetos (clases, herencia, encapsulación), incorporan herramientas propias de la programación funcional. Esto permite a los desarrolladores combinar la organización robusta de los objetos con la flexibilidad y expresividad de las funciones lambda y los flujos de datos (streams), eligiendo la mejor herramienta para cada problema específico.

La expresión "ciudadanos de primera clase" significa que las funciones son tratadas como cualquier otra variable o valor en el lenguaje. Esto implica que una función puede ser asignada a una variable, pasada como argumento a otra función o devuelta como resultado de un procedimiento. En C, esto se lograba limitadamente con punteros, pero en el paradigma funcional, esta capacidad es nativa y central para la construcción de abstracciones de alto nivel.


## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis de una función lambda en Java se compone principalmente de tres partes: los parámetros, el operador de flecha (->) y el cuerpo de la función. La estructura general sigue el patrón (parámetros) -> { cuerpo }. Dependiendo de la complejidad de la lógica y la cantidad de argumentos, esta sintaxis puede simplificarse considerablemente para mejorar la legibilidad del código.

Si la función solo recibe un parámetro, los paréntesis son opcionales. Asimismo, si el cuerpo de la lambda consiste en una única instrucción que devuelve un valor, no es necesario utilizar llaves ni la palabra clave return, ya que el compilador infiere el retorno automáticamente. En casos de lógica más compleja, se requieren llaves y una instrucción de retorno explícita, comportándose de manera similar a un método convencional pero sin nombre.

Un aspecto crucial es que Java utiliza la inferencia de tipos, por lo que no suele ser necesario declarar los tipos de los parámetros de entrada, ya que se deducen del contexto de la interfaz funcional a la que se asigna la lambda. No obstante, si se desea mayor claridad, los tipos pueden especificarse explícitamente entre paréntesis. Esta flexibilidad permite que el código funcional en Java sea mucho más limpio que las antiguas implementaciones mediante clases anónimas.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

El concepto de pasar una función como parámetro permite crear métodos altamente reutilizables cuya lógica interna no está "cableada", sino que se inyecta en el momento de la ejecución. Este tipo de métodos se denominan funciones de orden superior. El método transformar actúa como un molde que delega la operación específica sobre la cadena a la función que recibe por argumento.

En Java, para recibir una función, se debe declarar el parámetro utilizando una interfaz funcional, en este caso Function<String, String>. Dentro del método, la lógica se ejecuta invocando el método apply(). En JavaScript, al ser las funciones ciudadanos de primera clase sin necesidad de interfaces formales, el parámetro se invoca directamente como si fuera el nombre de la función.

Ejemplo en JavaScript:

````JavaScript
const transformar = (cadena, funcionTransformadora) => {
    return funcionTransformadora(cadena);
};

const aMayusculas = s => s.toUpperCase();
console.log(transformar("hola mundo", aMayusculas));
````
Ejemplo en Java:

````Java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> f) {
        return f.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        System.out.println(transformar("java", aMayusculas));
    }
}
````


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Una de las mayores utilidades de las lambdas es la capacidad de definir comportamiento "al vuelo" (on the fly) sin necesidad de almacenar la función en una variable intermedia. Esto resulta ideal para operaciones puntuales que no se repetirán en otras partes del código. Al pasar la lambda directamente como argumento, el código se vuelve más declarativo y se reduce el ruido visual.

En este caso, la lógica de inversión se define en el mismo lugar donde se llama al método transformar. En Java, se utiliza la clase StringBuilder para realizar la inversión de manera eficiente, mientras que en JavaScript se suele recurrir al encadenamiento de métodos sobre arreglos. En ambos lenguajes, la firma del método receptor permanece idéntica, demostrando la flexibilidad del desacoplamiento entre el método y la operación.

Ejemplo en JavaScript:

````JavaScript
console.log(transformar("espejo", s => s.split('').reverse().join('')));
````
Ejemplo en Java:

````Java
System.out.println(transformar("espejo", 
    s -> new StringBuilder(s).reverse().toString()));
````


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o closure es una función que "captura" o recuerda el entorno léxico en el que fue creada. Esto significa que la función lambda puede acceder a variables locales definidas en su ámbito exterior, incluso si la ejecución de ese ámbito ya ha finalizado. Es una herramienta poderosa para parametrizar funciones basándose en datos que solo están disponibles en el momento de la definición de la lambda.

En Java, existe una restricción técnica importante: las variables locales capturadas por una lambda deben ser finales o efectivamente finales. Esto implica que el valor de la variable no puede ser modificado después de su inicialización si va a ser usada dentro de la lambda. Esta limitación garantiza la seguridad en entornos multihilo, evitando condiciones de carrera al asegurar que el valor capturado sea inmutable.

````Java
public class Main {
    public static void main(String[] args) {
        // Variable local en el ámbito exterior
        String prefijo = "Hola, "; 
        
        // La lambda captura la variable 'prefijo' (es una closure)
        Function<String, String> saludar = s -> prefijo + s;
        
        // Si intentáramos hacer: prefijo = "Adiós, "; el compilador daría error.
        
        System.out.println(transformar("Mundo", saludar));
    }

    public static String transformar(String texto, Function<String, String> f) {
        return f.apply(texto);
    }
}
````

/*
static Function<Double, Double> crearDescuento (int porcentaje){
    return cantidad -> cantidad * (1 - porcentaje/100.0)
}
main(){
    Function <Double, Double> descuento25 = crearDescuuento(25);
    Function <Double, Double> descuento50 = crearDescuuento(50);

    double descontado = descuento25.apply(1000);
}

*/

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La principal diferencia radica en el estado. Un puntero a función en C es simplemente una dirección de memoria; no tiene capacidad nativa para transportar datos adicionales del contexto donde fue creado. Por el contrario, una lambda es un objeto (o una estructura similar) que encapsula tanto el código como el entorno (clausura). Mientras que el puntero es solo lógica pura, la lambda combina lógica y datos capturados.

Otra diferencia fundamental es el nivel de abstracción y seguridad. En C, trabajar con punteros a funciones implica lidiar con direcciones de memoria directamente, lo que es propenso a errores de segmentación si no se gestionan correctamente los tipos. En Java, las lambdas están integradas en el sistema de tipos a través de interfaces funcionales, proporcionando una capa de seguridad en tiempo de compilación y una sintaxis mucho más legible y menos propensa a errores de bajo nivel.

Finalmente, la gestión de memoria es distinta. En Java, las lambdas se gestionan mediante el recolector de basura (Garbage Collector), encargándose de liberar la memoria de los datos capturados cuando la lambda ya no es necesaria. En C, el programador debe asegurar manualmente que el contexto al que pudiera referenciar indirectamente un puntero (como variables globales o memoria dinámica) siga siendo válido durante la invocación.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

La capacidad de una función para devolver otra función es una técnica avanzada de programación funcional conocida como currificación o fábricas de funciones. El método crearDescuento no realiza el cálculo del precio final inmediatamente, sino que genera y devuelve una lógica personalizada de descuento que podrá ser aplicada repetidamente sobre diferentes valores numéricos.

En este ejemplo, la closure se produce porque la función lambda devuelta "encierra" el valor del parámetro porcentaje. Aunque el método crearDescuento termine su ejecución, la función lambda resultante conserva acceso a ese valor específico de porcentaje para realizar sus cálculos futuros. Cada vez que se llama a la fábrica, se crea un nuevo entorno cerrado con un valor de porcentaje distinto.

````Java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // El parámetro 'porcentaje' queda capturado en la closure
        return precio -> precio - (precio * porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuentoIVA = crearDescuento(21.0);
        Function<Double, Double> descuentoBlackFriday = crearDescuento(50.0);

        double precioBase = 100.0;
        System.out.println("Precio con IVA: " + descuentoIVA.apply(precioBase));
        System.out.println("Precio Black Friday: " + descuentoBlackFriday.apply(precioBase));
    }
}
````


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional es una interfaz en Java que contiene exactamente un único método abstracto. Aunque puede contener múltiples métodos predeterminados (default) o estáticos, es la presencia de un solo método por implementar lo que permite al compilador de Java realizar la correspondencia entre una expresión lambda y el tipo de la interfaz. Este concepto es conocido técnicamente como SAM (Single Abstract Method).

El requisito fundamental es la unicidad del método abstracto para evitar ambigüedades. Para facilitar su identificación y evitar que accidentalmente se añadan más métodos abstractos en el futuro, se suele utilizar la anotación @FunctionalInterface. Aunque no es obligatoria para que el código funcione, actúa como un contrato que el compilador verifica, emitiendo un error si la interfaz no cumple con la estructura requerida.

Este mecanismo es el puente que permite a Java, un lenguaje estrictamente tipado, integrar lambdas sin introducir un nuevo sistema de tipos complejo. Al asignar una lambda a una interfaz funcional, Java trata el bloque de código como una implementación anónima de ese único método abstracto, permitiendo que la programación funcional conviva con la infraestructura de objetos existente.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Definir una interfaz funcional personalizada permite dar nombres con significado semántico a las operaciones, lo que mejora la legibilidad del dominio del problema. En lugar de usar tipos genéricos como Function, el uso de Transformador indica claramente la intención del código en el contexto de procesamiento de textos.

La interfaz debe marcarse con la anotación @FunctionalInterface y declarar el método que reciba y devuelva un String. Una vez definida, cualquier lambda que acepte una cadena y retorne otra podrá ser asignada a una variable de tipo Transformador.

````Java
@FunctionalInterface
interface Transformador {
    String procesar(String entrada);
}

public class Main {
    public static void main(String[] args) {
        Transformador aMayusculas = s -> s.toUpperCase();
        Transformador exclamar = s -> s + "!!!";

        System.out.println(aMayusculas.procesar("hola"));
        System.out.println(exclamar.procesar("atención"));
    }
}
````


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
