<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se representa de forma muy natural incrustando una estructura dentro de otra. Así, un Punto puede modelarse con dos coordenadas, x e y, y una Linea puede modelarse como una estructura que contiene exactamente dos Punto: el origen y el destino. Esa relación expresa claramente un “línea tiene dos puntos”.

Para completar el ejemplo, conviene definir una función para calcular la distancia entre dos puntos y otra para obtener la longitud de una línea. La segunda puede reutilizar la primera, lo que evita duplicar lógica y hace el código más claro. La longitud de una línea no es más que la distancia entre sus dos extremos.
````c++
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx * dx + dy * dy);
}

double longitud(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea linea = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la linea: %.2f\n", longitud(linea));

    return 0;
}
````

````java
    class Punto{
        private double x, y;

        public Punto(double x, double y){
            this...;
        }

        //encapsuulado dentro de Punto metemos la fuuncion distancia entre puntos

        public distancia(Punto p1){
            return Math.sqrt(...)
        }
    }

    class Linea{
        private Punto p1, p2;

        public Linea(Punto p1, Punto p2){
            ...
        }
        public longitudDeLaLinea(){
            return this.p1.distancia(this.p2);
        }
    }
````


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la misma idea se expresa mediante clases. La clase Punto encapsula sus coordenadas y ofrece un método para calcular la distancia a otro Punto. La clase Linea contiene dos objetos Punto y ofrece un método para calcular su longitud. Aquí ya no se trabaja con datos “abiertos” como en C, sino con estado privado y comportamiento asociado.

La ocultación de información permite además imponer inmutabilidad. Para ello, los atributos se declaran private final, no se ofrecen métodos de modificación y todos los valores se fijan en el constructor. De este modo, una vez creado un Punto, sus coordenadas no cambian; y una vez creada una Linea, tampoco cambian sus extremos. En C eso depende de disciplina; en Java puede hacerse formar parte del diseño.
````java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser null");
        }
        this.inicio = inicio;
        this.fin = fin;
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
````


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad indica cuántas instancias de una clase pueden estar relacionadas con una instancia de otra clase. Es una forma de expresar la cantidad permitida en cada extremo de una relación. En orientación a objetos resulta muy útil para describir si una relación es uno a uno, uno a muchos, opcional, obligatoria, etc.

En el ejemplo de Linea y Punto, de Linea a Punto la multiplicidad es 2, porque toda línea está formada por exactamente dos puntos. En cambio, de Punto a Linea la multiplicidad no está fijada por el modelo: un mismo punto podría no pertenecer a ninguna línea, a una sola o a muchas. Por tanto, en esa dirección sería 0..*, salvo que se impusiera alguna restricción adicional en el diseño.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte se da cuando el objeto contenido forma parte esencial del objeto contenedor y su ciclo de vida queda ligado a él. Si desaparece el contenedor, también desaparecen sus componentes. Es la idea más estricta de “parte de”. Un ejemplo típico sería una casa y sus habitaciones entendidas como elementos inseparables del todo.

La composición débil, en cambio, se da cuando el contenedor simplemente mantiene referencias a otros objetos, pero estos pueden existir independientemente. Aquí no hay dependencia de ciclo de vida. A esta relación se la suele llamar agregación o asociación en sentido más laxo, mientras que el nombre composición propiamente dicho suele reservarse para la composición fuerte. En resumen: composición fuerte implica pertenencia y vida ligada; composición débil implica relación sin destrucción conjunta.

*/ 
Fuerte: el contentedor es el que crea los objetos que contiene
Debil: el contenedor y contenido tienen ciclos de vida independientes
/*

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase usa otra recibiéndola como parámetro, devolviéndola como resultado, creándola dentro de un método con new o empleándola como variable local, normalmente no se habla de composición, sino de dependencia. La idea clave es que esa otra clase interviene en la implementación o en la interfaz, pero no pasa necesariamente a formar parte del estado permanente del objeto.

La composición aparece cuando una clase mantiene referencias o componentes como parte de su estructura interna estable. En cambio, la dependencia describe un uso puntual o instrumental. Dicho de forma simple: si una clase “tiene” objetos de otra como parte de sí misma, hay composición; si solo “usa” otra clase para hacer algo, hay dependencia. Es una diferencia muy importante en modelado, aunque al principio ambas se parezcan más de lo que conviene.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

En una composición fuerte, Linea puede guardar directamente las coordenadas dentro de puntos propios creados por ella misma. Así, aunque conceptualmente existan puntos, desde fuera no se comparte ningún objeto Punto preexistente con el exterior. Los puntos quedan ligados a la vida de la línea, porque se crean para ella y no se reutilizan como entidades independientes.

En una composición débil, en cambio, Linea recibe dos objetos Punto ya existentes y guarda sus referencias. En ese caso, los puntos pueden seguir existiendo aunque desaparezca la línea, e incluso pueden compartirse entre varias líneas. Esa es precisamente la diferencia de ciclo de vida entre ambos diseños.

````java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}
// Composición fuerte
public final class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
// Composición débil
public final class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser null");
        }
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
````

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java no se destruyen objetos explícitamente como en C++ con destructores ni como en C con free. Cuando en composición fuerte un contenedor deja de ser accesible, también dejan de ser accesibles sus componentes si no existen otras referencias hacia ellos. En ese momento pasan a ser candidatos para la recolección de basura.

Por eso no se observa que Linea destruya de forma explícita sus Punto. Simplemente no hace falta. Java delega la liberación de memoria al recolector de basura, que elimina los objetos inaccesibles automáticamente. Dicho de otro modo: la destrucción lógica la marca la pérdida de referencias; la destrucción física de memoria la realiza el runtime. El programador se ahorra trabajo, aunque a cambio pierde control exacto sobre el momento concreto.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Aquí interesa una composición débil, porque los Profesor pueden existir independientemente del Departamento. El departamento mantiene una colección de profesores y además una referencia a uno de ellos como director. La invariante importante es que siempre haya director y que ese director pertenezca a la lista de profesores. Eso obliga a validar cuidadosamente el constructor, el cambio de director y la eliminación de profesores.

Para no romper la encapsulación, no debe devolverse el array interno ni exponerse su tamaño real de implementación. En su lugar, se ofrece una interfaz controlada: saber cuántos profesores hay, obtener uno por posición, añadir al final y eliminar por posición. La lógica delicada aparece al borrar: no puede eliminarse al director si antes no se ha nombrado otro, porque eso dejaría al departamento en un estado inválido.

````java
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre invalido");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
public class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final String nombre;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre de departamento invalido");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir director inicial");
        }

        this.nombre = nombre;
        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;
        this.director = directorInicial;
    }

    public String getNombre() {
        return nombre;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posicion invalida");
        }
        return profesores[pos];
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("Profesor null");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben mas profesores");
        }
        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("Director null");
        }
        if (!contiene(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posicion invalida");
        }
        if (numProfesores == 1) {
            throw new IllegalStateException("No puede quedarse sin profesores ni sin director");
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No puede eliminarse al director actual");
        }

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    private boolean contiene(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }
        return false;
    }
}
````

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al usar List, desaparece gran parte del trabajo mecánico de gestión del array: no hace falta llevar un contador manual, ni desplazar elementos al borrar, ni comprobar capacidad máxima salvo que se quiera imponer aparte. La colección ya resuelve de forma interna esas tareas repetitivas. El código queda más corto, más expresivo y bastante menos propenso a errores clásicos de índices.

Respecto a devolver todos los profesores de una vez, sería un error exponer directamente la lista interna, porque desde fuera podría modificarse sin pasar por las validaciones del departamento. Eso rompería la encapsulación y permitiría violar la invariante del director. La forma correcta es devolver una copia o una vista no modificable, para que el exterior pueda consultar pero no alterar la estructura interna “a traición”, que es una especialidad muy poco elegante del caos.
````java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private final String nombre;
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre de departamento invalido");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir director inicial");
        }

        this.nombre = nombre;
        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public String getNombre() {
        return nombre;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("Profesor null");
        }
        profesores.add(profesor);
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("Director null");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        director = nuevoDirector;
    }

    public void removeProfesor(int pos) {
        Profesor profesor = profesores.get(pos);

        if (profesores.size() == 1) {
            throw new IllegalStateException("No puede quedarse sin profesores ni sin director");
        }
        if (profesor == director) {
            throw new IllegalStateException("No puede eliminarse al director actual");
        }

        profesores.remove(pos);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
````

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Una composición recursiva aparece cuando una clase contiene referencias a objetos de su misma clase. En este caso, una Persona puede tener una madre que también es una Persona. El diseño recursivo es muy natural para modelar árboles genealógicos, estructuras jerárquicas o expresiones anidadas. Para mantener la inmutabilidad, los atributos deben ser privados y finales, sin métodos de modificación.

La recursividad permite encadenar relaciones de forma elegante: nieto → madre → abuela. Otros ejemplos clásicos de composición recursiva son los nodos de una lista enlazada, los nodos de un árbol binario, los directorios que contienen subdirectorios o las excepciones con causa encadenada. Son estructuras que se describen casi solas una vez que se acepta la idea de “un objeto de este tipo contiene otro del mismo tipo”.
````java
public final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre invalido");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Ana", abuela);
        Persona nieto = new Persona("Luis", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre: " + nieto.getMadre().getNombre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
    }
}
````

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Una relación bidireccional es aquella en la que ambas clases mantienen referencia mutua y puede navegarse en ambos sentidos. En lugar de tener solo “el departamento conoce a sus profesores”, también se tendría “cada profesor conoce a su departamento”. Eso facilita ciertos accesos, pero complica el mantenimiento de la consistencia, porque cada cambio debe reflejarse en los dos lados.

En el ejemplo de Profesor y Departamento, habría que añadir en Profesor un atributo que apunte a su Departamento y actualizar ambas partes de forma coordinada al añadir, eliminar o trasladar profesores. Si se añade un profesor al departamento, el profesor debe pasar a apuntar a ese departamento; si se elimina, debe dejar de apuntar a él. Además, convendría impedir estados incoherentes, por ejemplo que un departamento crea tener a un profesor que, por su lado, diga pertenecer a otro distinto. Cuando hay bidireccionalidad, el verdadero enemigo no es el compilador: es el despiste.

````java
public class Profesor {
    private final String nombre;
    private Departamento departamento;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre invalido");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    void setDepartamento(Departamento departamento) {
        this.departamento = departamento;
    }
}
// Fragmentos relevantes dentro de Departamento
public void addProfesor(Profesor profesor) {
    if (profesor == null) {
        throw new IllegalArgumentException("Profesor null");
    }
    if (profesor.getDepartamento() != null) {
        throw new IllegalArgumentException("El profesor ya pertenece a un departamento");
    }
    profesores.add(profesor);
    profesor.setDepartamento(this);
}

public void removeProfesor(int pos) {
    Profesor profesor = profesores.get(pos);

    if (profesor == director) {
        throw new IllegalStateException("No puede eliminarse al director actual");
    }

    profesores.remove(pos);
    profesor.setDepartamento(null);
}
````