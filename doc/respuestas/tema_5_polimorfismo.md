<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es la capacidad que poseen los objetos de responder de manera distinta a un mismo mensaje o llamada a un método, dependiendo de su tipo real en tiempo de ejecución. En la programación orientada a objetos, este mecanismo sirve para desacoplar el código que utiliza un objeto de la implementación específica de dicho objeto, permitiendo escribir algoritmos genéricos que funcionen con cualquier clase que pertenezca a una misma jerarquía.

La sobreescritura (overriding) es el mecanismo técnico que permite implementar el polimorfismo. Consiste en redefinir en una subclase un método que ya ha sido declarado en una superclase. Para que sea una sobreescritura válida, el método en la subclase debe mantener la misma firma (nombre, parámetros y tipo de retorno) que el de la clase base, sustituyendo o extendiendo su comportamiento original.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica es el proceso mediante el cual el sistema determina, durante la ejecución del programa y no en la compilación, qué implementación exacta de un método debe ejecutarse. Se produce cuando se invoca un método sobre una referencia de una superclase que apunta a una instancia de una subclase. Es el pilar fundamental del polimorfismo, pues sin este enlace en tiempo de ejecución, el programa siempre ejecutaría el método definido por el tipo de la variable (el tipo estático) en lugar del tipo real del objeto (el tipo dinámico).

En Java, la ligadura dinámica es el comportamiento por defecto para todos los métodos no estáticos, no finales y no privados; no es necesario indicarlo de forma explícita. En contraste, en C++ (lenguaje con el que se tiene experiencia previa) se debe indicar explícitamente mediante el uso de la palabra clave virtual en la función de la clase base para permitir este comportamiento; de lo contrario, se aplica ligadura estática.

En lenguajes como Python, al ser lenguajes con tipado dinámico, la ligadura es intrínsecamente dinámica. No existe el concepto de ligadura estática para métodos de objetos, ya que la resolución de nombres ocurre siempre en tiempo de ejecución buscando en el diccionario de la instancia y de sus clases base.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.
````java
class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo para el desminado.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe, usa el saludo por defecto del Soldado
}

public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = { new Zapador(), new Artillero() };
        
        for (Soldado s : peloton) {
            s.saludar(); // Polimorfismo en acción
        }
    }
}
````


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es posible invocar el método de la clase base desde el método sobreescrito. Esto resulta de gran utilidad cuando no se desea sustituir el comportamiento original por completo, sino complementarlo o realizar una especialización conservando la lógica común definida en la superclase.

Para lograr esto en Java se utiliza la palabra clave super. Esta palabra actúa como una referencia a la instancia actual pero tratada como un objeto de la superclase inmediata, permitiendo el acceso a sus métodos y constructores.

````Java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Llama al saludo normal del Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
````

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescritar en Java, se debe mantener estrictamente la misma lista de parámetros. El tipo de retorno debe ser el mismo o un subtipo del original (retorno covariante). Además, el método sobreescrito no puede reducir la visibilidad del método original (por ejemplo, de public a private) ni lanzar excepciones nuevas o más generales que las declaradas en la clase base.

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) radica en que la primera ocurre en clases distintas relacionadas por herencia y mantiene la firma, mientras que la segunda ocurre en la misma clase y consiste en definir métodos con el mismo nombre pero con distinta lista de parámetros. La sobrecarga es un polimorfismo en tiempo de compilación (estático), mientras que la sobreescritura lo es en tiempo de ejecución (dinámico).

La anotación @Override es una instrucción para el compilador que indica la intención de sobreescritar un método. Es altamente recomendable usarla siempre porque evita errores sutiles: si se comete un error tipográfico en el nombre del método o en los parámetros, el compilador generará un error al no encontrar un método coincidente en la superclase para sobreescritar.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Efectivamente, el polimorfismo de inclusión se utiliza en Java de forma inherente desde el momento en que se comienza a trabajar con clases. Dado que en Java toda clase hereda implícitamente de la clase raíz Object, cualquier sobrescritura de sus métodos, como toString(), equals() o hashCode(), constituye una aplicación práctica de este concepto. No es necesario declarar estructuras complejas para que el mecanismo polimórfico entre en juego.

Cuando se redefine el método toString(), se está proporcionando una implementación específica para una subclase. Si posteriormente se trata a una instancia de dicha subclase mediante una referencia de tipo Object y se invoca al método, la Máquina Virtual de Java (JVM) determinará en tiempo de ejecución que debe ejecutar la versión sobrescrita y no la original de la clase Object. Este comportamiento, donde una misma llamada a un método produce resultados distintos según el objeto real que la ejecuta, es la base del polimorfismo.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta se define como una clase incompleta que sirve exclusivamente como modelo o base para otras clases. Se declara utilizando la palabra clave abstract y su propósito principal es capturar atributos y comportamientos comunes que serán compartidos por una jerarquía, pero que no tienen sentido por sí mismos de forma aislada. Por esta razón, no es posible crear instancias (objetos) de una clase abstracta directamente mediante el operador new.

Por su parte, un método abstracto es aquel que se declara en la clase base pero carece de implementación (no tiene cuerpo). Actúa como un contrato que obliga a las subclases no abstractas a proporcionar su propia lógica específica para dicho método. En el diseño de sistemas, esto garantiza que todos los objetos de una jerarquía respondan a los mismos mensajes, aunque lo hagan de formas diferentes.

Para el ejemplo propuesto del Soldado, el diseño se estructuraría de la siguiente manera:

````Java
public abstract class Soldado {
    public void saluda() {
        System.out.println("¡A la orden!");
    }

    // El método atacar es abstracto porque cada soldado ataca de forma distinta
    public abstract void atacar();
}

class Infante extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El infante dispara su fusil.");
    }
}
````

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final actúa como un restrictor de herencia y modificación. Cuando se aplica a una clase, se impide que cualquier otra clase pueda heredar de ella, cerrando la jerarquía de forma definitiva. Si se aplica a un método, se permite la herencia del mismo, pero se prohíbe terminantemente su sobrescritura en las subclases, asegurando que el comportamiento definido originalmente permanezca inalterado.

La relación con el polimorfismo es de oposición directa. El polimorfismo de inclusión depende de la capacidad de las subclases para redefinir comportamientos. Al marcar un método o una clase como final, se está anulando la posibilidad de aplicar despacho dinámico sobre esos elementos, ya que no existirán versiones alternativas del método para ejecutar en tiempo de ejecución. Es una herramienta de diseño utilizada para garantizar la seguridad o la integridad de la lógica de negocio.

En la API estándar de Java, un ejemplo clásico de clase final es java.lang.String. Java prohíbe heredar de String por razones de seguridad y eficiencia técnica, evitando que se pueda alterar el comportamiento básico de las cadenas de texto, las cuales son fundamentales para el funcionamiento del lenguaje y el manejo de memoria (como el String Pool).


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java se entienden como tipos de referencia que definen un contrato puro de comportamiento. A diferencia de las clases abstractas, que pueden contener estado (variables de instancia) y lógica parcial, las interfaces se limitan tradicionalmente a declarar métodos que deben ser implementados por las clases que las adopten. Representan lo que un objeto "puede hacer", más que lo que el objeto "es".

Aunque guardan similitudes con las clases abstractas en el sentido de que ninguna puede ser instanciada y ambas permiten el uso de polimorfismo, su naturaleza es distinta. Una clase abstracta define una relación de identidad ("es un"), mientras que una interfaz define una capacidad o rol ("actúa como"). Esta distinción es crucial para mantener un diseño desacoplado y flexible.

Una de las mayores potencias de las interfaces es que una clase puede implementar múltiples interfaces de forma simultánea. Esto permite superar la limitación de la herencia simple de clases en Java, permitiendo que un objeto adquiera comportamientos de distintos orígenes sin necesidad de una jerarquía lineal rígida. Es una forma de polimorfismo múltiple basada en tipos.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Para resolver este requerimiento, se define una estructura donde la clase base Punto establece la firma del cálculo de distancia, delegando la lógica matemática a las implementaciones específicas de 2D y 3D. El uso de instanceof asegura la integridad del cálculo, verificando que los tipos sean compatibles antes de proceder con el downcasting.

````Java
public abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting seguro
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        throw new IllegalArgumentException("El punto debe ser de tipo Punto2D");
    }
}

class Linea {
    private Punto inicio, fin;

    public Linea(Punto p1, Punto p2) {
        this.inicio = p1;
        this.fin = p2;
    }

    public double obtenerLongitud() {
        // Se aplica polimorfismo: Linea no sabe si los puntos son 2D o 3D
        return inicio.calcularDistanciaA(fin);
    }
}
````
Este diseño permite que la clase Linea sea totalmente agnóstica a la dimensionalidad de los puntos. Gracias al polimorfismo, el método obtenerLongitud() invocará automáticamente la versión correcta de calcularDistanciaA en tiempo de ejecución, permitiendo que el sistema sea extensible a nuevas dimensiones (como un Punto3D) sin modificar el código de la clase Linea.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces permite que una interfaz herede las declaraciones de métodos de otra interfaz utilizando la palabra clave extends. Esto facilita la creación de jerarquías de comportamiento, donde una interfaz hija puede refinar o ampliar las capacidades de una interfaz padre. Es un mecanismo de especialización de contratos muy común en el diseño de APIs complejas.

A diferencia de la herencia de clases, en Java sí existe la herencia múltiple de interfaces. Una interfaz puede extender varias interfaces simultáneamente, combinando sus contratos en uno solo. Esto no genera los conflictos de ambigüedad típicos de la herencia múltiple de clases (como el problema del diamante en C++), ya que originalmente las interfaces no contenían implementaciones de métodos, solo declaraciones.

El ejemplo de los ficheros ilustra cómo se pueden acumular responsabilidades de forma jerárquica:

````Java
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible "es un" Fichero y algo más
interface FicheroEscribible extends Fichero {
    void escribirContenido(String datos);
    void eliminar();
}

class DocumentoTexto implements FicheroEscribible {
    @Override
    public String leerContenido() { return "Datos del archivo..."; }

    @Override
    public void escribirContenido(String datos) { /* Lógica de escritura */ }

    @Override
    public void eliminar() { /* Lógica de borrado */ }
}
````