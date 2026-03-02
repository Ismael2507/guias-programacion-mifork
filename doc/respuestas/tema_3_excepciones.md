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
            double resultado=Calculadora.raiz(num);
            sout(resultado);
        }
    }
            
## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

