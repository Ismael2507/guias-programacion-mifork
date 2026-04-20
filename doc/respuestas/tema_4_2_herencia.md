<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia en orientación a objetos es un mecanismo por el cual una clase (subclase) adquiere propiedades y comportamientos de otra (superclase). Se expresa con la relación “A es-un B”: por ejemplo, Artillero es-un Soldado. Esto implica dos ideas clave. Primero, la compatibilidad de tipos: cualquier objeto de una subclase puede tratarse como si fuera de la superclase. Segundo, la herencia de estado y comportamiento: la subclase reutiliza atributos y métodos de la superclase.

La compatibilidad permite escribir código genérico. Por ejemplo, un array de Soldado puede almacenar objetos Artillero y Zapador. La herencia de comportamiento implica que ambos subtipos comparten el método saludar() definido en Soldado, además del atributo nombre.
````java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

// Uso
Soldado[] ejercito = {
    new Artillero("Luis", 5),
    new Zapador("Ana", 3)
};

for (Soldado s : ejercito) {
    s.saludar();
}
````

Composicion - tiene un/varios
Herencia    - es un

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase, se ejecutan todos los constructores de la jerarquía, desde la superclase hasta la subclase. El orden es de arriba hacia abajo: primero el constructor de la superclase (Soldado) y luego el de la subclase (Artillero o Zapador). Esto asegura que la parte heredada del objeto quede correctamente inicializada antes de completar la subclase.

La palabra clave super dentro de un constructor sirve para invocar explícitamente el constructor de la superclase. Es obligatoria cuando la superclase no tiene un constructor sin parámetros accesible, ya que Java no puede inferir cómo inicializarla automáticamente.

Si la superclase no dispone de constructor por defecto visible, entonces sí, debe llamarse a super(...) siempre pasando los argumentos necesarios. De lo contrario, el compilador generará un error porque no sabrá cómo construir la parte heredada del objeto.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Un objeto Artillero contiene internamente tanto sus propios atributos como los heredados de Soldado, incluido el nombre.

Sin embargo, esto no implica que puedan usarse directamente desde el código de la subclase. El modificador private restringe el acceso exclusivamente a la clase donde se declara. Por tanto, aunque el dato exista en memoria, no es accesible directamente en la subclase.

Por ejemplo, en Artillero no se puede hacer System.out.println(nombre);. Para acceder a ese dato, sería necesario usar métodos públicos/protegidos definidos en Soldado, como un getter o métodos que lo utilicen (como saludar()).



## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos mejora la extensibilidad, ya que permite añadir nuevas subclases sin modificar el código existente que trabaja con el tipo base. Esto sigue el principio de “abierto/cerrado”: abierto a extensión, cerrado a modificación.

Por ejemplo, si se añade un nuevo tipo Medico, no es necesario cambiar el código que recorre el array de Soldado, porque sigue siendo compatible:
````java
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}

Soldado[] ejercito = {
    new Artillero("Luis", 5),
    new Zapador("Ana", 3),
    new Medico("Carlos")
};

for (Soldado s : ejercito) {
    s.saludar(); // No cambia
}
````
El código que usa la abstracción (Soldado) permanece intacto, lo que reduce errores y facilita mantenimiento.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, se puede tener una referencia del supertipo (Soldado) que apunte a objetos reales de subtipos (Artillero, Zapador). Esto se denomina upcasting y ocurre de forma automática. Sin embargo, con esa referencia solo se pueden invocar métodos definidos en el tipo declarado (Soldado).

Para acceder a métodos específicos del subtipo, se necesita un downcasting, que consiste en convertir la referencia al tipo real. Esto debe hacerse con cuidado, comprobando previamente el tipo con instanceof para evitar errores en ejecución.
````java
for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
````
instanceof permite verificar el tipo real del objeto antes de hacer el casting, evitando excepciones como ClassCastException.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido (protected) permite que un atributo o método sea accesible desde la propia clase, sus subclases y otras clases del mismo paquete. Es un punto intermedio entre private y public.

En Java se implementa usando la palabra clave protected. Esto es útil cuando se quiere permitir que las subclases accedan directamente a ciertos datos sin exponerlos completamente.
````java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre, int minas) {
        super(nombre);
    }

    public void ponerMinas() {
        System.out.println(nombre + " está poniendo minas");
    }
}
````
Aquí Zapador puede usar directamente nombre gracias al acceso protegido.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

No ocurre en todos los lenguajes, aunque es una característica común en muchos lenguajes modernos.

En general: Algunos lenguajes (como C++) no tienen una raíz única obligatoria; puedes crear jerarquías de clases totalmente independientes. Otros, denominados lenguajes de "jerarquía de raíz única", fuerzan a que toda clase herede de un ancestro común.

En Java: Sí existe una clase base universal llamada Object (java.lang.Object). Si una clase no extiende explícitamente a otra, el compilador hace que herede automáticamente de Object. Esto garantiza que todos los objetos compartan métodos críticos como equals(), hashCode(), toString() y getClass().

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es la capacidad de una clase de heredar comportamientos y atributos de más de una superclase simultáneamente.

En Java: No existe la herencia múltiple de clases. Una clase solo puede extender (extends) una única clase padre. Esto se diseñó así para evitar problemas de ambigüedad, como el "Problema del Diamante" (donde un nieto no sabe qué implementación de un método usar si sus dos padres heredan de un mismo abuelo).

Matiz: Java permite la herencia múltiple de tipos a través de las interfaces, ya que una clase puede implementar múltiples interfaces.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

Para que sea no controlada, la clase debe heredar de RuntimeException. Aquí tienes la implementación con la composición del objeto Usuario y la sobrecarga de constructores para incluir la causa:

````Java
public class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

    // Constructor con mensaje y usuario
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Constructor con mensaje, usuario y causa (Throwable)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
````
## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia establece una relación semántica de tipo "Es un" (is-a). Si usas herencia solo para "aprovechar" métodos de otra clase que no tiene una relación lógica con la tuya, creas un acoplamiento artificial.

Si la superclase cambia en el futuro para adaptarse a su propio dominio, podrías romper la lógica de la subclase que solo quería "copiar" código.

La herencia debe reflejar una jerarquía conceptual, no ser un atajo de teclado.

/*No usar solo por reutilizar codigo
    Debe usuarse cuando se necesita la compatibilidad de tipos

Usar herencia implica uun gran acoplamiento desde la clase derivada hacia la clase base
    La clase derivada depende mucho de la base
    Cambios internos en la clase base podrian llegar a afectar a la derivada

*/

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La composición representa una relación de "Tiene un" (has-a). Se favorece porque:

Flexibilidad: Permite cambiar el comportamiento en tiempo de ejecución (puedes cambiar el objeto compuesto por otro), mientras que la herencia es estática (se define en tiempo de compilación).

Menor acoplamiento: Las clases dependen de interfaces o contratos simples, no de la estructura interna completa de una jerarquía de clases.

Diseño más limpio: Evita las jerarquías profundas y complejas que son difíciles de mantener.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Se refiere a que la subclase depende de los detalles de implementación de la superclase para funcionar correctamente.

Si una superclase cambia un detalle interno (incluso si no cambia su interfaz pública), puede alterar el comportamiento de la subclase de forma inesperada (efectos secundarios).

Las subclases suelen tener acceso a miembros protected, lo que significa que la "caja negra" de la superclase ya no es tan cerrada para ellas. El "contrato" entre ambas es demasiado íntimo.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Opción A: Herencia (Relación "Es un")
````Java
public class Persona {
    protected String dni;
    protected String nombre;
}

public class Estudiante extends Persona {
    private String carnet;
}

public class Trabajador extends Persona {
    private String nss;
}
````
Opción B: Composición (Relación "Tiene un")
````Java
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

public class Estudiante {
    private DatosPersonales datos; // Composición

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

public class Trabajador {
    private DatosPersonales datos; // Composición

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
````