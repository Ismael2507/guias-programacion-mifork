<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

La encapsulación consiste en agrupar datos y operaciones relacionadas dentro de una misma unidad lógica, llamada objeto. Esto permite ocultar detalles internos y exponer solo lo necesario, reduciendo dependencias y errores. En lenguajes como Java, se apoya en los modificadores de visibilidad.

La abstracción permite centrarse en qué hace un objeto y no en cómo lo hace. Se trata de modelar conceptos del problema real mediante clases que representan solo las características relevantes, simplificando la complejidad del sistema.

La herencia permite crear nuevas clases a partir de otras existentes, reutilizando código y ampliando o modificando comportamientos. Esto favorece la extensibilidad y evita duplicaciones, algo que en C se debe simular manualmente.

El polimorfismo permite tratar objetos de distintas clases relacionadas de forma uniforme, normalmente a través de una interfaz común. Esto posibilita que una misma operación se comporte de forma distinta según el objeto concreto que la implemente.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java es uno de los lenguajes orientados a objetos más extendidos, diseñado desde su origen con este paradigma como eje central. Todo el código se estructura en clases y objetos, lo que lo convierte en un buen punto de partida para aprender POO.

C++ permite programación orientada a objetos como una extensión de C, incorporando clases, herencia y polimorfismo. Sin embargo, la orientación a objetos es opcional y convive con programación procedural.

Python es un lenguaje multiparadigma en el que todo es un objeto, incluidos los tipos básicos. Ofrece una sintaxis más flexible y dinámica, lo que facilita la experimentación con conceptos de POO.

C# es un lenguaje moderno orientado a objetos, similar a Java en muchos aspectos, ampliamente utilizado en el ecosistema de Microsoft. Incorpora POO de forma estricta junto con características más recientes.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada se basa en dividir el programa en bloques de control bien definidos, utilizando secuencias, selecciones y bucles. Su objetivo principal es mejorar la legibilidad y mantenibilidad del código frente al uso indiscriminado de saltos como goto.

Este paradigma introduce funciones o procedimientos para agrupar instrucciones relacionadas, pero los datos suelen estar separados del código que los manipula. En C, este enfoque es el más habitual.

La programación modular va un paso más allá, organizando el programa en módulos independientes que encapsulan funcionalidad concreta. Cada módulo expone una interfaz pública y oculta su implementación interna.

Aunque la programación modular mejora la organización y reutilización, no integra completamente datos y comportamiento como lo hace la POO. Esa integración es precisamente uno de los saltos conceptuales clave de las clases y objetos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define, en primer lugar, por su estado, que viene representado por los valores de sus atributos. Estos atributos equivalen a las variables asociadas a una estructura concreta.

En segundo lugar, un objeto tiene un comportamiento, definido por los métodos que puede ejecutar. Dichos métodos operan sobre el estado interno del objeto y representan acciones relacionadas con él.

Finalmente, un objeto posee una identidad, que lo distingue de otros objetos aunque tengan el mismo estado. Dos objetos distintos pueden contener los mismos valores, pero seguir siendo entidades separadas en memoria.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o definición que describe cómo serán los objetos de un determinado tipo. En ella se especifican los atributos y métodos que tendrán los objetos creados a partir de esa clase.

Un objeto no es lo mismo que una clase. El objeto es una entidad concreta creada a partir de una clase, mientras que la clase es solo la definición. A este proceso de creación se le llama instanciación, y el objeto resultante es una instancia de la clase.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma forma. Existen lenguajes basados en prototipos, como JavaScript, donde los objetos pueden crearse directamente sin clases tradicionales.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En Java, los objetos se almacenan dinámicamente en una zona de memoria conocida como heap. Las variables que se usan en el código no contienen el objeto en sí, sino una referencia a él.

Esto no es igual en todos los lenguajes. En C++, por ejemplo, un objeto puede almacenarse tanto en la pila como en el heap, dependiendo de cómo se cree. Java elimina esa elección para simplificar la gestión de memoria.

La recolección de basura es un mecanismo automático que libera la memoria ocupada por objetos que ya no son accesibles. El programador no libera memoria manualmente, a diferencia de C, lo que reduce ciertos errores pero resta control directo.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función asociada a una clase u objeto. Define un comportamiento que puede ejecutarse sobre los datos del objeto, teniendo acceso directo a sus atributos.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una clase, pero con distinta lista de parámetros. El compilador decide cuál usar según los argumentos proporcionados.

Esto permite ofrecer distintas formas de realizar una misma operación, mejorando la expresividad del código. En C, este comportamiento no existe de forma nativa y debe simularse con nombres distintos.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

    class Punto {
    double x;
    double y;

        double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
        }
    }

    class Prueba {
        public static void main(String[] args) {
            Punto p = new Punto();
            p.x = 3;
            p.y = 4;
            System.out.println(p.calculaDistanciaAOrigen());
        }
    }

Una clase en Java define atributos y métodos de forma conjunta. En este ejemplo, la clase Punto modela un punto en el plano mediante dos coordenadas.

El método calculaDistanciaAOrigen opera directamente sobre los atributos del objeto, sin necesidad de pasarlos como parámetros. Esto ilustra la unión entre datos y comportamiento.

La creación del objeto se realiza con new, y la llamada al método se hace a través de la instancia. Este estilo contrasta con C, donde los datos y funciones suelen estar separados.

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa Java es el método main. Este método es el primero que se ejecuta cuando se lanza la aplicación desde la máquina virtual.

La palabra clave static indica que el método pertenece a la clase y no a una instancia concreta. Por eso puede ejecutarse sin crear ningún objeto previamente.

static no se utiliza solo en main, sino también para atributos y métodos compartidos por todas las instancias. Combinado con final, permite definir constantes asociadas a la clase.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar un programa Java desde la línea de comandos se utiliza javac, que genera un fichero .class. Para ejecutarlo se emplea java, indicando el nombre de la clase con main.

Java es un lenguaje compilado, pero no a código máquina nativo. El resultado de la compilación es byte-code, un código intermedio independiente de la plataforma.

La máquina virtual de Java es la encargada de ejecutar ese byte-code. Esto permite que el mismo programa funcione en distintos sistemas sin recompilarlo.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

La palabra clave new se utiliza para crear un nuevo objeto en memoria. Reserva espacio para el objeto y devuelve una referencia a él.

Un constructor es un método especial que se ejecuta al crear un objeto. Sirve para inicializar el estado inicial del objeto y tiene el mismo nombre que la clase.

    class Empleado {
        String dni;
        String nombre;
        String apellidos;

        Empleado(String dni, String nombre, String apellidos) {
            this.dni = dni;
            this.nombre = nombre;
            this.apellidos = apellidos;
        }
    }


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es una referencia al objeto actual sobre el que se está ejecutando un método. Permite distinguir entre atributos del objeto y variables locales con el mismo nombre.

No se llama igual en todos los lenguajes. En C++ también se utiliza this, mientras que en otros lenguajes puede variar o ser implícito.

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
Este método recibe otro objeto Punto como parámetro. Se calcula la distancia entre ambos utilizando sus atributos respectivos.

El uso de this deja claro cuál es el punto que invoca el método y cuál es el punto recibido como argumento.

Este enfoque es más natural que pasar múltiples coordenadas sueltas, como se haría en programación procedural.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, los parámetros se pasan por valor, pero en el caso de objetos el valor copiado es la referencia. Esto significa que ambos apuntan al mismo objeto en memoria.

Si se modifica el estado del objeto dentro del método, el cambio es visible fuera. Sin embargo, si se reasigna la referencia, eso no afecta al exterior.

En cambio, tipos primitivos como int se copian completamente. Si se modifica un entero dentro del método, el valor original no cambia fuera de él.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

toString() es un método que devuelve una representación en texto del objeto. En Java, todas las clases lo heredan de una clase base común.

Este método existe en otros lenguajes con conceptos similares, aunque el nombre puede variar. Su finalidad principal es facilitar la depuración y la salida por pantalla.

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Una clase se parece a un struct en cuanto a que ambos agrupan datos relacionados. Sin embargo, el struct carece de comportamiento asociado de forma natural.

En C, las funciones que operan sobre un struct están separadas y no tienen acceso implícito a sus datos. Tampoco existe control de visibilidad.

Para que un struct se pareciera a una clase, necesitaría encapsulación, métodos asociados y un mecanismo similar a this.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

    typedef struct {
        double x;
        double y;
    } Punto;

    double distanciaOrigen(Punto *p) {
        return sqrt(p->x * p->x + p->y * p->y);
    }
Aquí, el struct agrupa los datos, pero la función está separada. El parámetro Punto* actúa como una versión explícita de this.

La principal diferencia es que el programador debe pasar manualmente la referencia al objeto. En Java, este proceso es implícito y más seguro.

Esta comparación muestra que la POO no introduce magia, sino una organización más estructurada y coherente del mismo concepto.
