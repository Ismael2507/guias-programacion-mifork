<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Para lograr una estructura de datos capaz de almacenar cualquier tipo de elemento sin conocerlo a priori, se recurre al tipo más genérico disponible en el lenguaje. En C, esto se logra mediante punteros genéricos void*, mientras que en Java se utiliza la clase Object, dado que es la superclase de la que heredan todas las demás clases (polimorfismo de inclusión).

Al definir un array interno de tipo Object, la estructura de datos puede aceptar cualquier instancia que se le pase, ya que el compilador realiza un upcasting automático (conversión hacia arriba en la jerarquía) desde el tipo específico hasta Object.

````Java
public class ContenedorGenerico {
    private Object[] elementos;
    private int tamaño;

    public ContenedorGenerico(int capacidad) {
        elementos = new Object[capacidad];
        tamaño = 0;
    }

    public void añadir(Object elemento) {
        if (tamaño < elementos.length) {
            elementos[tamaño++] = elemento;
        }
    }

    public Object obtener(int indice) {
        return elementos[indice];
    }
}
````


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir algoritmos y estructuras de datos de manera independiente de los tipos concretos sobre los que operan. El objetivo principal es maximizar la reutilización del código sin sacrificar la seguridad de tipos, permitiendo que una misma implementación funcione de manera idéntica para enteros, cadenas de texto o estructuras complejas.

El ejemplo anterior, empleando Object o void*, representa efectivamente una forma primitiva o básica de programación genérica. Logra el objetivo de reutilización, ya que el mismo código sirve para almacenar cualquier elemento. Sin embargo, se considera un enfoque rudimentario y peligroso porque delega toda la responsabilidad de la gestión de tipos al tiempo de ejecución, perdiendo las garantías que ofrece el compilador.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema de emplear Object en Java (o void* en C/C++) radica en la pérdida de información sobre el tipo original en tiempo de compilación. Cuando se extrae un elemento de la estructura de datos, el compilador solo sabe que es un Object. Para utilizar los métodos específicos del elemento original, es obligatorio realizar una conversión explícita o downcasting (por ejemplo, (String) elemento).

Esta necesidad de hacer downcasting rompe la seguridad de tipos estática. Si por error se introduce un tipo inesperado en la estructura (por ejemplo, un Integer en un contenedor donde se esperan instancias de String), el compilador no detectará ninguna anomalía.

El fallo se manifestará exclusivamente en tiempo de ejecución. En C/C++, un cast incorrecto desde void* puede derivar en un comportamiento indefinido o corrupción de memoria. En Java, el error es controlado pero igualmente fatal, provocando el lanzamiento de una excepción ClassCastException que interrumpe el flujo normal del programa.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son variables temporales utilizadas en la declaración de clases, interfaces o métodos, que representan un tipo de dato que será especificado más adelante. En lugar de utilizar variables para almacenar valores, los parámetros de tipo operan a nivel de compilación para "almacenar" tipos (como Integer, String o Soldado). Convencionalmente se representan con letras mayúsculas simples, como <T> (Type), <E> (Element) o <K, V> (Key, Value).

El uso de parámetros de tipo permite que la estructura de datos mantenga un registro exacto del tipo que está manejando. Cuando el programador instancia la clase, sustituye el parámetro de tipo por un tipo concreto (argumento de tipo). De este modo, el compilador puede verificar que todas las operaciones de inserción y extracción coincidan con ese tipo concreto, automatizando el casting y detectando errores de incompatibilidad antes de ejecutar el programa.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En ambos lenguajes, la programación genérica permite crear contenedores tipados de forma segura, evitando conversiones manuales al extraer los elementos. A continuación se muestra la implementación en C++ mediante templates, donde se utiliza std::vector para almacenar std::string.

````C++
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación del template en C++
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");

    // Recorrido seguro, el tipo es std::string
    for (const std::string& elemento : lista) {
        std::cout << elemento << " tiene " << elemento.length() << " letras." << std::endl;
    }
    return 0;
}
````
Por otro lado, en Java se emplean generics sobre las colecciones estándar, como ArrayList. El funcionamiento a nivel de código fuente es análogo, garantizando que el método length() de la clase String pueda ser invocado de inmediato sin necesidad de hacer un cast desde Object.

````Java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Instanciación de generics en Java
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");

        // Recorrido seguro, el compilador sabe que son String
        for (String elemento : lista) {
            System.out.println(elemento + " tiene " + elemento.length() + " letras.");
        }
    }
}
````

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Aunque la sintaxis de templates en C++ y generics en Java es similar, el funcionamiento interno del compilador es radicalmente distinto. En C++, se emplea la instanciación de plantillas. Cuando se declara un std::vector<std::string>, el compilador de C++ genera físicamente una copia completa del código de la clase vector, sustituyendo el parámetro de tipo genérico por std::string. Si luego se usa un std::vector<int>, se compilará otra clase completamente nueva. Esto garantiza un rendimiento extremo, pero aumenta el tamaño del código ejecutable.

En cambio, Java emplea un mecanismo llamado type erasure (borrado de tipos). Para mantener la compatibilidad hacia atrás con versiones antiguas de Java, el compilador genera una única clase compilada (un solo bytecode). Durante la compilación, se verifican todos los tipos para asegurar la corrección del código. Una vez verificados, el compilador "borra" los parámetros de tipo <T> y los reemplaza por Object (o por el límite superior, si lo hay).

Finalmente, el compilador de Java inserta automáticamente los casts ocultos ((String)) en los lugares donde se extraen datos, de manera invisible para el programador. En consecuencia, en tiempo de ejecución, la Máquina Virtual de Java no sabe si una lista era de String o de Integer; solo ve una lista de Object.




## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Para alojar valores de tipos diferentes, se define una clase con múltiples parámetros de tipo, separados por comas, como <T, U>. Esto otorga flexibilidad para que cada elemento del par mantenga su propia identidad de tipo estricta.

````Java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
````
Este tipo de estructura es especialmente útil en Java para sortear la limitación de que los métodos solo pueden retornar un único valor. A continuación, se muestra cómo se utilizaría la clase Par para retornar dos cálculos numéricos simultáneamente desde una función.

````Java
public class Estadistica {
    // Función que devuelve un Par con dos valores Double
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double suma = 0.0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;
        
        double sumaDiferencias = 0.0;
        for (double d : datos) sumaDiferencias += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaDiferencias / datos.length);
        
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] array = {2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0};
        Par<Double, Double> resultado = calcularMediaYDesviacion(array);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
````


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Un método genérico permite parametrizar su comportamiento independientemente de si la clase que lo contiene es genérica o no. La declaración del parámetro de tipo <T> se coloca justo antes del tipo de retorno del método.

````Java
import java.util.Random;

public class Utilidades {
    // Solución con Object (Sin generics)
    public static Object seleccionaUnoObject(Object a, Object b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }

    // Solución con Generics
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }
}
````
La diferencia principal radica en las garantías en tiempo de compilación. Con la versión Object, se pueden pasar dos parámetros de tipos totalmente incompatibles (por ejemplo, un String y un Integer), y el compilador lo permitirá, forzando además a realizar un downcasting peligroso sobre el valor devuelto.

Con la versión genérica <T>, se logran dos ventajas fundamentales: (i) el método devuelve exactamente el mismo tipo T que se introdujo, eliminando la necesidad de downcasting; y (ii) el compilador infiere el tipo a partir de los argumentos y fuerza a que ambos objetos sean compatibles con ese mismo tipo T. Si se intenta pasar un texto y un número, el compilador emitirá un error.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, es posible restringir los tipos que un parámetro genérico puede aceptar utilizando "límites" (bounds). En Java, esto se logra mediante la sintaxis <T extends ClaseBase>, lo que asegura que el tipo introducido herede de una clase o implemente una interfaz específica.

A continuación, se presenta la solución basada en polimorfismo puro (sin generics), usando la superclase Number de la API de Java, seguida de la solución con generics acotados.

````Java
// Solución 1: Sin Generics, usando polimorfismo clásico
public class PuntoClasico {
    private Number x;
    private Number y;

    public PuntoClasico(Number x, Number y) {
        this.x = x; this.y = y;
    }
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double calcularDistanciaA(PuntoClasico otro) {
        return Math.sqrt(Math.pow(x.doubleValue() - otro.getX().doubleValue(), 2) +
                         Math.pow(y.doubleValue() - otro.getY().doubleValue(), 2));
    }
}


// Solución 2: Con Generics y restricción (Bounds)

    public class PuntoGen<T extends Number> {
    private T x;
    private T y;

    public PuntoGen(T x, T y) {
        this.x = x; this.y = y;
    }
    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(PuntoGen<T> otro) {
        return Math.sqrt(Math.pow(x.doubleValue() - otro.getX().doubleValue(), 2) +
                         Math.pow(y.doubleValue() - otro.getY().doubleValue(), 2));
    }
}
````
Respecto al "type erasure", dado que se ha establecido una restricción, el compilador ya no sustituye <T> por Object. El tipo final tras la compilación será el límite superior definido; en este caso, la clase Number. El bytecode resultante de la clase PuntoGen será internamente casi idéntico al de la clase PuntoClasico.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

El refuerzo del chequeo de tipos altera de forma drástica el modo de interactuar con la clase. En la solución sin generics (clase PuntoClasico), ambas coordenadas son referencias a la superclase Number. Esto permite una mezcla de subtipos; se puede instanciar pasando un Integer para la 'x' y un Double para la 'y' sin ningún problema. Al llamar a getX(), el método siempre devolverá una referencia de tipo Number, obligando al programador a investigar qué subtipo numérico concreto se está manejando si se requieren operaciones específicas no definidas en Number.

Por el contrario, la solución con generics acotados (PuntoGen<T extends Number>) fuerza una homogeneidad estricta. Al instanciar un PuntoGen<Integer>, el compilador exige que tanto 'x' como 'y' sean obligatoriamente enteros. No se permite mezclar un Integer con un Double en la misma instancia bajo el tipo T estricto (a menos que se declarara de forma imprecisa como PuntoGen<Number>).

Además, al invocar getX() en la solución genérica, el método devuelve directamente el tipo exacto con el que se instanció (por ejemplo, devuelve Integer), eliminando ambigüedades y preservando toda la funcionalidad original del dato sin necesidad de conversiones.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para resolver este problema de diseño común, se debe parametrizar la propia interfaz con un tipo <T>. Este parámetro representará el tipo específico de la clase que va a implementar la interfaz. Al hacerlo, el método distanciaA ya no recibe un Punto genérico, sino exactamente el tipo T.

Cuando se crea la clase Punto2D, se implementa la interfaz sustituyendo el parámetro genérico por la propia clase Punto2D (implements Punto<Punto2D>). De este modo, la sobreescritura del método exigirá obligatoriamente un parámetro Punto2D a nivel de compilación, haciendo redundantes e innecesarios el instanceof y el downcasting.

````Java
// Interfaz parametrizada
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

// La clase se pasa a sí misma como argumento de tipo
public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p2d) { 
        // No hay necesidad de instanceof ni downcasting
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }
    
    @Override 
    public double distanciaA(Punto3D p3d) { 
        return Math.sqrt(Math.pow(x - p3d.x, 2) + 
                         Math.pow(y - p3d.y, 2) + 
                         Math.pow(z - p3d.z, 2)); 
    }
}
````


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

No, List<String> no es subtipo de List<Object>. Los tipos genéricos en Java son estrictamente invariantes. Esto significa que si A es subtipo de B (ej. String de Object), un contenedor genérico Generico<A> no guarda relación de herencia con Generico<B>. Esta decisión se tomó para proteger la memoria: si se permitiera que una lista de textos fuera vista como una lista de objetos, se podrían añadir números en ella, provocando un fallo catastrófico en el código que espera extraer textos de esa lista.

Por el contrario, los arrays primitivos en Java (creados en las primeras versiones del lenguaje) son covariantes. Esto significa que String[] sí es considerado un subtipo de Object[]. Al permitir esto, Java introduce un problema de seguridad en tiempo de ejecución: es posible tomar un array de String, asignarlo a una referencia de Object[], e insertar un Integer. El compilador lo permitirá, pero la Máquina Virtual lanzará un ArrayStoreException durante la ejecución, al intentar guardar un número en un array construido internamente para textos.

En base a estos comportamientos, las propiedades se definen así respecto a la relación entre Contenedor<Tipo>:

Covariante: Si A hereda de B, entonces Contenedor<A> es subtipo de Contenedor<B>. Permite lectura segura, pero escritura peligrosa.

Contravariante: Invierte la relación de herencia. Si A hereda de B, entonces Contenedor<B> se comporta como un subtipo lógico de Contenedor<A>. Permite escritura segura.

Invariante: No existe ninguna relación jerárquica entre los contenedores, independientemente de la relación entre los tipos internos.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard o comodín (?) es un operador en los generics de Java que representa un "tipo desconocido". Se emplea para relajar la invariancia estricta de las clases genéricas y permitir la covarianza y contravarianza de forma controlada y segura en los parámetros de los métodos, rigiéndose por el principio PECS (Producer Extends, Consumer Super).

El comodín ? extends T (covarianza) especifica que se acepta cualquier tipo que sea T o una subclase de T. Se utiliza cuando la estructura de datos actúa como productora (se van a leer elementos de ella). Es seguro leerlos porque se tiene la garantía de que, sea cual sea el tipo exacto, será al menos un T. Sin embargo, el compilador prohíbe añadir elementos (excepto null), ya que no conoce el tipo exacto de la lista y se podría violar su integridad.

El comodín ? super T (contravarianza) especifica que se acepta cualquier tipo que sea T o una superclase de T. Se usa cuando la estructura actúa como consumidora (se van a insertar elementos en ella). Es seguro añadir instancias de tipo T porque la lista receptora fue definida para albergar T o ancestros de T. No obstante, la lectura está limitada: cualquier elemento leído es recuperado únicamente como Object.

````Java
import java.util.List;

public class EjemploWildcards {
    // (i) Productor: Lee de la lista. Covarianza usando ? extends.
    // Acepta List<Number>, List<Integer>, List<Double>...
    public static double calcularSuma(List<? extends Number> lista) {
        double suma = 0.0;
        for (Number n : lista) { // Es seguro leer como Number
            suma += n.doubleValue();
        }
        // lista.add(10); // ERROR DE COMPILACIÓN: no se puede asegurar el tipo real.
        return suma;
    }

    // (ii) Consumidor: Escribe en la lista. Contravarianza usando ? super.
    // Acepta List<Integer>, List<Number>, List<Object>...
    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10); // Seguro: un Integer cabe en una lista de Integer, Number u Object
        lista.add(20);
        // Integer valor = lista.get(0); // ERROR: get devuelve Object, no garantiza Integer
    }
}
````