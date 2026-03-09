<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C no existen excepciones, por lo que el error debe indicarse mediante algún mecanismo explícito. Una primera opción consiste en devolver un valor especial que indique error. Por ejemplo, si la función calcula la raíz cuadrada de un número positivo, podría devolver -1 cuando el parámetro sea negativo, asumiendo que ese valor no es válido como resultado.

    #include <stdio.h>
    #include <math.h>

    double raiz(double x) {
        if (x < 0) {
            return -1.0;  // valor especial de error
        }
        return sqrt(x);
    }

    int main() {
        double r = raiz(-4);
        if (r < 0) {
            printf("Error: número negativo\n");
        } else {
            printf("Resultado: %f\n", r);
        }
    }

Una segunda opción consiste en devolver un código de error y pasar el resultado por referencia mediante un puntero. De esta forma se separa claramente el resultado correcto del estado de error.

    #include <stdio.h>
    #include <math.h>

    int raiz(double x, double* resultado) {
        if (x < 0) {
            return 0;  // indica error
        }
        *resultado = sqrt(x);
        return 1;      // indica éxito
    }

    int main() {
        double r;
        if (!raiz(-4, &r)) {
            printf("Error: número negativo\n");
        } else {
            printf("Resultado: %f\n", r);
        }
    }

En ambos diseños el control del error se realiza fuera de la función raiz, pero obliga al programador a comprobar manualmente el estado tras cada llamada.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo del lenguaje que permite señalar que se ha producido una situación anómala durante la ejecución de un programa. No forma parte del flujo normal de la lógica del programa, sino que representa un evento excepcional que impide continuar de manera habitual.

El objetivo de las excepciones es separar claramente el código que realiza la tarea principal del código que gestiona los errores. Cuando se implementa una función, se puede indicar que en determinadas condiciones se lanzará una excepción. Cuando se llama a esa función, se puede decidir capturarla y tratarla o dejar que se propague. Esto mejora la claridad, la modularidad y la mantenibilidad del código.

Excepcion: Surge en situaciones atipicas
Cuando implementamos nos permite indicar mas claramente el error
Cuando llamamos me facilita separar la logica normal de la de reaccion o manejo de la situacion erronea

    class Calculadora{
        public static double raiz (double raiz){
            if(num<0.0){
                throw new IllegalArgumentException("num negativo")
            }else{
                return Math.sqrt(num);
            }
        }
    }

    class App{
        main(){
            double num=leerTeclado();
            
            try{
            double resultado=Calculadora.raiz(num);
            sout(resultado);
            }catch(IllegalArgumentException e){
                sout("El numero es negativo, no te preocupes, nadie es perfecto")
            }
        }
    }
            
## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, las excepciones forman parte del lenguaje. Se puede definir una clase Calculadora con un método que lance una excepción si el número es negativo.

public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = raiz(-4);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

Aquí el método raiz no devuelve un valor especial. Si el argumento es negativo, se lanza una excepción. El método main controla la excepción mediante un bloque try-catch, lo que permite informar al usuario desde fuera del método.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa crear un objeto excepción y señalar que se ha producido un error mediante la palabra clave throw. Capturar o controlar una excepción significa interceptarla mediante un bloque catch para tratarla de forma específica.

Si una función lanza una excepción y no la captura, esta se propaga hacia la función que la llamó. El proceso continúa subiendo por la pila de llamadas hasta encontrar un manejador adecuado. Mientras se propaga, las funciones intermedias se van terminando abruptamente.

En el ejemplo anterior, si raiz lanza la excepción y main la captura, el flujo salta directamente al catch. Las funciones que no la capturan no se reanudan después; su ejecución termina en el punto donde ocurrió la excepción.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación automática a través de la pila evita tener que comprobar manualmente códigos de error tras cada llamada, como ocurre en C. El flujo normal del programa queda más limpio y centrado en la lógica principal.

Además, permite que el nivel más adecuado decida cómo tratar el error. Una función intermedia puede no saber cómo reaccionar, pero una capa superior sí. Esto mejora la separación de responsabilidades y reduce el acoplamiento entre módulos.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En orientación a objetos, las excepciones son objetos. En Java, todas heredan de la clase Exception o RuntimeException. Esto permite encapsular información relevante dentro del objeto.

Gracias a la encapsulación, una excepción puede contener un mensaje, un código de error o incluso otros datos adicionales. También es posible crear excepciones personalizadas heredando de Exception.
```java
    public class NumeroNegativoException extends Exception {
        public NumeroNegativoException(String mensaje) {
            super(mensaje);
        }
    }

```
## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Un objeto excepción suele contener al menos un mensaje descriptivo del error y la traza de la pila (stack trace). Esta traza indica en qué método y en qué línea se produjo el problema.

A diferencia del ejemplo en C, donde solo se devuelve un valor o código, aquí se dispone de información estructurada sobre el contexto del error. Esto resulta muy útil cuando el manejador necesita diagnosticar el problema o registrarlo.

Un mensaje: getMessage()
La traza de la pila: getStackTrace(), printStackTrace()
Opcionalmente, la "Causa", otra excepcion que es la verdadera causa

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java es posible tener varios bloques catch asociados a un mismo bloque try. Cada bloque catch está diseñado para capturar un tipo concreto de excepción, lo que permite tratar de forma distinta diferentes errores que puedan producirse dentro del try. De esta forma, el programador puede especificar manejadores especializados para cada clase de problema.

Cuando ocurre una excepción dentro del bloque try, el sistema busca entre los bloques catch aquel cuyo tipo de excepción sea compatible con la excepción lanzada. Esta búsqueda se realiza en orden, desde el primer catch hacia abajo. El primer bloque que coincida con el tipo de la excepción será el que se ejecute.

Sin embargo, solo se ejecuta un único bloque catch. Una vez que una excepción ha sido capturada por un manejador, los demás bloques catch asociados al mismo try se ignoran. Después de ejecutar ese catch, el flujo del programa continúa en la primera instrucción situada después del conjunto try-catch.
```java
    try {
        double r = Calculadora.raiz(-9);
        System.out.println(r);
    } catch (IllegalArgumentException e) {
        System.out.println("Argumento inválido: " + e.getMessage());
    } catch (ArithmeticException e) {
        System.out.println("Error aritmético");
    } catch (Exception e) {
        System.out.println("Otro tipo de error");
    }
```
En este ejemplo se definen tres posibles manejadores. Si el método raiz lanza una IllegalArgumentException, se ejecutará únicamente el primer catch. Los otros bloques no se ejecutarán, aunque su tipo también pudiera ser compatible con excepciones distintas.

/* Se puede tener mas de uno
- Se va comprobando por orden hasta el primero que encaje.
- Hay que poner del mas especifico al mas general
*/

IOExceotion <- AccessDeniedException


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Cuando se produce una excepción, el flujo normal del programa se interrumpe y puede comenzar su propagación por la pila de llamadas. Sin embargo, en muchos casos es necesario ejecutar siempre ciertas acciones antes de abandonar un bloque de código, como cerrar ficheros, liberar recursos o finalizar operaciones pendientes. En Java esto se consigue utilizando el bloque finally.

El bloque finally se ejecuta siempre, independientemente de que ocurra o no una excepción dentro del try. También se ejecuta tanto si la excepción se captura con un catch como si continúa propagándose hacia niveles superiores. Su finalidad es garantizar la ejecución de código de limpieza o liberación de recursos, incluso cuando la ejecución normal del programa ha sido interrumpida.

Un ejemplo con catch podría ser el siguiente. Aquí se intenta realizar una operación que puede fallar, se captura la excepción si ocurre, y el bloque finally asegura que se ejecute el código de cierre.
```java
    try {
        System.out.println("Abriendo recurso...");
        
        double r = Calculadora.raiz(-9);  // puede lanzar excepción
        System.out.println("Resultado: " + r);

    } catch (IllegalArgumentException e) {
        System.out.println("Error: " + e.getMessage());

    } finally {
        System.out.println("Cerrando recurso (siempre se ejecuta)");
    }
```

/*
El finally se ejecuta siempre que entremos en el try.
*/

También es posible utilizar finally sin bloque catch. En este caso, si ocurre una excepción, el código del finally se ejecuta y después la excepción continúa propagándose al método llamador.

    try {
        System.out.println("Abriendo recurso...");

        double r = Calculadora.raiz(-9);  // lanza excepción
        System.out.println("Resultado: " + r);

    } finally {
        System.out.println("Cerrando recurso (siempre se ejecuta)");
    }

En este segundo caso no se captura la excepción en ese punto, pero se garantiza que el código del finally se ejecute antes de que la excepción siga propagándose. Esto permite asegurar que los recursos se liberen correctamente incluso en situaciones de error.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java el bloque finally puede utilizarse sin un bloque catch. La estructura mínima permitida es try seguido de finally. En este caso no se captura ninguna excepción en ese punto, pero se garantiza que el código del finally se ejecute antes de que el control abandone el bloque try, incluso si se ha producido una excepción que continuará propagándose hacia el método llamador.

El bloque finally se ejecuta tanto si ocurre una excepción como si no ocurre. Si el código del try termina normalmente, se ejecuta el finally antes de continuar con las instrucciones posteriores. Si ocurre una excepción, primero se ejecuta el finally y después la excepción sigue propagándose si no ha sido capturada.

También se ejecuta el finally aunque exista un return dentro del try. Cuando se alcanza el return, Java prepara el valor de retorno pero antes de abandonar el método ejecuta el código del bloque finally. Solo después de ejecutar ese bloque se produce realmente el retorno del método.
```java
public static int ejemplo() {
    try {
        System.out.println("Dentro del try");
        return 10;
    } finally {
        System.out.println("Se ejecuta el finally antes de salir del método");
    }
}
```
En este ejemplo, al llamar al método se imprimirá primero el mensaje del try y después el del finally. Solo tras ejecutar el finally el método devolverá el valor 10. Esto demuestra que el bloque finally se ejecuta incluso cuando el flujo del programa abandona el try mediante un return.

/*
Si, puede ir sin catch. Se ejecuta, puesto que es finally y si hay excepcion, como no tenemos catch, se propaga.
*/

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java existen dos grandes categorías de excepciones: controladas (checked exceptions) y no controladas (unchecked exceptions). Las excepciones controladas son aquellas que el compilador obliga a manejar explícitamente. Si un método puede producir una de estas excepciones, el programador debe capturarla con try-catch o declararla con throws en la firma del método. Este mecanismo fuerza a que el programador tenga en cuenta ciertas situaciones de error previsibles.

Las excepciones no controladas son aquellas que no requieren ser declaradas ni capturadas obligatoriamente. Estas excepciones heredan de RuntimeException. El compilador no exige que se gestionen porque suelen representar errores de programación o situaciones que normalmente no deberían ocurrir si el código está correctamente escrito. Aunque pueden capturarse, no es obligatorio hacerlo.

La clase RuntimeException actúa como base para la mayoría de las excepciones no controladas. Todas las excepciones que heredan de ella se consideran excepciones que pueden producirse durante la ejecución normal del programa sin que el compilador obligue a tratarlas. Ejemplos típicos incluyen NullPointerException o ArithmeticException.

Ejemplos de excepciones controladas habituales (o que podría definir un programador):

    IOException (problemas al leer o escribir archivos).

    FileNotFoundException (archivo inexistente al intentar abrirlo).

    DatosInvalidosException (excepción personalizada para indicar datos incorrectos en una aplicación).

Ejemplos de excepciones no controladas:

    NullPointerException (uso de una referencia que vale null).

    ArithmeticException (errores aritméticos como división por cero).

    IndexOutOfBoundsException (acceso a posiciones inválidas en arrays o listas).

    IllegalArgumentException (argumentos incorrectos al llamar a un método).

Situaciones donde suele preferirse una excepción controlada:

    Fallos al acceder a archivos o recursos externos.

    Problemas al comunicarse con bases de datos o servicios externos.

    Errores de validación en datos que provienen del usuario o de entrada externa.

    Operaciones que dependen de recursos que pueden no estar disponibles.

Situaciones donde suele preferirse una excepción no controlada:

    Errores de programación que indican un uso incorrecto de una API.

    Parámetros inválidos pasados a un método por parte del propio programa.

    Accesos fuera de rango en estructuras de datos.

    Estados internos del programa que no deberían ocurrir si el diseño es correcto.


*/ IOException:
    AccessDeniedException
 /*

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

En Java, la palabra clave throws se utiliza en la declaración de un método para indicar que dicho método puede lanzar una o varias excepciones. Al incluir throws en la firma del método, se informa al compilador y a los programadores que utilicen ese método de que durante su ejecución podría producirse ese tipo de error. De este modo, el método no gestiona la excepción internamente, sino que declara que puede producirla.

El uso principal de throws aparece con las excepciones controladas (checked exceptions). Cuando un método puede generar una excepción de este tipo, el compilador obliga a que el programador haga una de dos cosas: capturarla con un bloque try-catch o declararla con throws para que se propague al método que realiza la llamada. Por tanto, throws forma parte del contrato del método y obliga al código que lo invoque a considerar esa posibilidad de error.

Se considera una alternativa a capturar la excepción porque permite no manejar el error en ese nivel del programa. En lugar de tratarlo inmediatamente, el método lo deja pasar hacia arriba en la pila de llamadas. Esto es útil cuando el método no dispone de suficiente contexto para decidir qué hacer ante el error, y es preferible que la decisión se tome en un nivel superior del programa.

Un ejemplo sencillo podría ser el siguiente:
````java
import java.io.*;

public class Ejemplo {

    public static void leerArchivo(String nombre) throws IOException {
        FileReader f = new FileReader(nombre);
        f.close();
    }

    public static void main(String[] args) {
        try {
            leerArchivo("datos.txt");
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
````

En este caso el método leerArchivo declara con throws IOException que puede producir esa excepción. El método main, que realiza la llamada, decide capturarla con un bloque try-catch y gestionar el error en ese punto.



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.
````java
public String leerFichero(Path p) throws IOException{

    try{
        ...
    }finally{
        ...
    }
}
````

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

En Java es posible incluir excepciones no controladas en la cláusula throws, por ejemplo aquellas que heredan de RuntimeException. El lenguaje no lo prohíbe, aunque no es obligatorio hacerlo. A diferencia de las excepciones controladas, el compilador no exige que estas excepciones aparezcan en la firma del método ni que se capturen explícitamente.

Cuando un método declara una excepción no controlada en throws, el método llamador no está obligado a capturarla con un try-catch. El programa compilará igualmente aunque no exista ningún bloque de captura. Esto ocurre porque las excepciones derivadas de RuntimeException se consideran errores que pueden producirse durante la ejecución normal del programa y cuya gestión no siempre es necesaria en cada punto del código.

Incluir una excepción no controlada en throws tiene principalmente un valor informativo o documental. Sirve para indicar a quien utilice el método que podría producirse cierta condición de error. De esta forma se comunica mejor el comportamiento del método, aunque el compilador no obligue a manejar esa situación.

El método llamador puede decidir capturar esa excepción si tiene sentido hacerlo en ese contexto. Por ejemplo, podría capturarse una IllegalArgumentException para mostrar un mensaje al usuario o registrar el error. Sin embargo, en muchos casos se deja que estas excepciones se propaguen hasta un nivel superior del programa, donde exista un manejador general o donde se decida terminar la ejecución.

*/ Por poder podemos, pero el compilador no va a obligar al bloque try.catch 
- No es habitual
- A veces se ponen por documentacion
/*


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda utilizar excepciones controladas cuando el error representa una situación previsible y recuperable, especialmente si depende del entorno o de elementos externos al programa. Es el caso de abrir un fichero, acceder a una base de datos o comunicarse con una red: son operaciones que pueden fallar aunque el código esté bien escrito. En esas situaciones tiene sentido que el compilador obligue a tener en cuenta el posible error, porque el método llamador quizá pueda tomar una decisión razonable, como reintentar, pedir otro nombre de fichero o informar al usuario.

Se suele preferir una excepción no controlada cuando el problema indica un uso incorrecto del método o un error de programación, no una circunstancia externa. IllegalArgumentException es el ejemplo típico: si un método recibe un argumento inválido, lo normal es considerar que el fallo está en quien llamó al método. En ese caso no suele tener mucho sentido forzar con el compilador a capturar la excepción en todos los lugares, porque lo adecuado es corregir el código. Dicho de forma simple: si el error “puede pasar en la realidad”, suele encajar mejor una controlada; si el error “no debería pasar si el programa está bien hecho”, suele encajar mejor una no controlada.

No en todos los lenguajes existen ambas opciones de la misma forma que en Java. Hay lenguajes que distinguen claramente entre excepciones controladas y no controladas, y otros en los que todas las excepciones funcionan prácticamente como no controladas. En C, directamente no hay excepciones integradas; en C++ existen excepciones, pero no una separación como la de Java; en Python tampoco existe una obligación del compilador de declarar o capturar ciertos tipos. Por tanto, el modelo de Java no es universal.

En los lenguajes donde solo existe una opción práctica, la más habitual es el modelo parecido al de las no controladas, es decir, excepciones que pueden lanzarse y capturarse, pero sin obligación de declararlas en la firma ni tratarlas explícitamente en cada llamada. Es un modelo más flexible y menos rígido, aunque también deja más responsabilidad al programador. Java aquí fue bastante especial: quiso forzar el tratamiento de ciertos errores, y por eso introdujo las excepciones controladas.

*/ 

/*


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido lanzar excepciones dentro de un catch. Un catch no está obligado a “resolver” el problema por completo; también puede usar la información de la excepción capturada para reaccionar y después lanzar otra excepción. Esto suele hacerse cuando se quiere traducir una excepción de bajo nivel a otra más adecuada al nivel de abstracción del programa. Por ejemplo, un error técnico de lectura de fichero puede convertirse en una excepción más cercana al dominio del problema, con un mensaje más útil para quien usa esa clase.

También se puede relanzar la misma excepción capturada usando throw e;. En ese caso no se crea una excepción nueva, sino que se deja que el error continúe propagándose después de haber hecho alguna acción previa, como registrar información, liberar recursos adicionales o dejar constancia en un log. Esto tiene sentido cuando en ese nivel no se puede solucionar el problema, pero sí interesa realizar una tarea intermedia antes de que otro método superior lo gestione.

Lanzar una nueva excepción dentro del catch tiene sentido cuando conviene ocultar detalles internos o presentar una interfaz más limpia. Relanzar la misma excepción tiene sentido cuando se quiere añadir contexto externo sin cambiar el error original. En ambos casos, el catch no “corta” necesariamente la propagación; simplemente puede modificar cómo continúa esa propagación.

Ejemplo de lanzar una nueva excepción dentro del catch:
````java
import java.io.*;

public class GestorFichero {

    public static void procesarFichero(String nombre) throws Exception {
        try {
            BufferedReader br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            throw new Exception("No se ha podido procesar el fichero de entrada", e);
        }
    }
}
````
Ejemplo de relanzar la misma excepción capturada:
````java
import java.io.*;

public class GestorFichero {

    public static void leerFichero(String nombre) throws IOException {
        try {
            BufferedReader br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            System.out.println("Se ha detectado un error de entrada/salida");
            throw e;   // se relanza la misma excepción
        }
    }
}
````
En el primer caso se cambia el tipo de excepción que se propaga, aportando una visión más abstracta del problema. En el segundo caso se conserva exactamente la misma excepción original, pero se aprovecha el catch para realizar una acción antes de que siga subiendo por la pila de llamadas.


*/
Si, tiene sentido
- Relanzar la misma excepcion
- Envolver en otra excepcion nueva (será causa)
- Lanzar otra excepcion totalmente nueva

/*


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

En Java se dice que una excepción es la “causa” de otra cuando una excepción original (de bajo nivel) provoca que se lance una nueva excepción de nivel más alto, conservando la referencia a la primera. Este mecanismo permite encapsular el error original dentro de otro más adecuado al nivel de abstracción del programa. Así se mantiene la información técnica del problema sin exponer directamente detalles internos a las capas superiores.

Este enfoque se utiliza cuando una clase detecta un error de bajo nivel (por ejemplo, un problema de entrada/salida) pero desea comunicar un error más significativo para el contexto de la aplicación. En lugar de perder la excepción original, se pasa como causa al constructor de la nueva excepción. De este modo se mantiene la relación entre ambas y se conserva toda la información útil para depuración.

Un ejemplo consiste en capturar una excepción IOException y envolverla dentro de una excepción personalizada que represente un problema lógico de la aplicación:
```java
import java.io.*;

class ErrorProcesamientoException extends Exception {
    public ErrorProcesamientoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class Procesador {

    public static void procesar(String fichero) throws ErrorProcesamientoException {
        try {
            BufferedReader br = new BufferedReader(new FileReader(fichero));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            throw new ErrorProcesamientoException(
                "No se pudo procesar el fichero", e);
        }
    }
}
```
En este caso, IOException es la causa de ErrorProcesamientoException. Si la excepción final se muestra por pantalla (por ejemplo con printStackTrace()), la causa también aparece. La traza normalmente muestra primero la excepción principal y después una sección indicando “Caused by”, seguida de la excepción original y su propia traza de pila. Esto permite ver tanto el error de alto nivel como el problema técnico que lo provocó.


*/
Causa excepcion:
- Se ve cuando la excepcion se muestra por pantalla
    "Excepcion externa (NetfluxException)
        ...
        
    Caused by excepcion interna (IOException)
        ...
    "
- Se puede obtener con el metodo getCause()
/*
