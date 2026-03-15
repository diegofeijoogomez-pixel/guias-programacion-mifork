# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C es posible construir estructuras más complejas componiendo unas con otras. La composición consiste en que una estructura contiene otras estructuras como parte de su definición, lo que suele describirse como “A tiene un B” o “A tiene varios B”. De esta forma, una estructura mayor se construye reutilizando estructuras más pequeñas que representan conceptos básicos del problema.

Por ejemplo, puede definirse una estructura Punto que represente una posición en el plano mediante dos coordenadas (x e y). A partir de ella, puede definirse una estructura Linea que esté compuesta por dos puntos: el punto inicial y el punto final. En este caso se diría que una línea tiene dos puntos. Además, pueden implementarse funciones auxiliares para calcular la distancia entre dos puntos y la longitud de una línea.

#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx*dx + dy*dy);
}

double longitud(struct Linea l) {
    return distancia(l.p1, l.p2);
}

En este ejemplo se observa claramente la composición: la estructura Linea contiene dos estructuras Punto. La función distancia calcula la distancia entre dos puntos aplicando la fórmula de la distancia euclídea, mientras que la función longitud obtiene la longitud de una línea utilizando dicha función sobre los dos puntos que la componen. De esta forma se reutiliza código y se organiza mejor la información del problema.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En orientación a objetos, la composición se expresa cuando una clase contiene objetos de otra clase como parte de su estado. En este caso, se puede modelar el problema definiendo una clase Punto que represente una posición en el plano y una clase Linea que esté formada por dos puntos. De esta forma, se mantiene la misma relación conceptual que en C: una línea tiene dos puntos. Sin embargo, en Java la composición se integra de manera natural dentro del diseño de clases.

Además, Java permite aplicar ocultación de información mediante modificadores de acceso. Si los atributos se declaran private y no se proporcionan métodos que los modifiquen, se puede garantizar que los objetos sean inmutables. En este ejemplo, las coordenadas de Punto se establecen únicamente en el constructor y no pueden cambiar posteriormente. Del mismo modo, una Linea se crea con dos puntos y no ofrece métodos para modificar esos puntos, por lo que la línea queda fijada una vez creada.

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

En este diseño se observa claramente la composición en orientación a objetos, ya que la clase Linea contiene dos objetos de la clase Punto. Gracias al uso de atributos private y final, junto con la ausencia de métodos modificadores, se garantiza que tanto los puntos como la línea sean inmutables. Esto mejora la seguridad y la fiabilidad del programa, ya que una vez creados los objetos no pueden cambiar su estado interno.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en la composición indica cuántas instancias de una clase pueden estar relacionadas con una instancia de otra clase. Es un concepto utilizado habitualmente en el modelado orientado a objetos (por ejemplo, en diagramas UML) para describir la cantidad mínima y máxima de objetos que pueden participar en una relación. De esta manera, la multiplicidad permite especificar si una relación es uno a uno, uno a muchos, muchos a uno o muchos a muchos.

En el ejemplo anterior, una Linea está compuesta exactamente por dos objetos Punto: uno representa el punto inicial y otro el punto final. Por tanto, la multiplicidad de Linea a Punto es 2, ya que cada instancia de Linea contiene exactamente dos instancias de Punto. No se permite que una línea tenga menos ni más puntos en este modelo concreto.

En la dirección contraria, la relación es diferente. Un mismo Punto podría pertenecer a ninguna, una o varias líneas, dependiendo de cómo se utilice dentro del programa. Por ejemplo, varias líneas podrían compartir un mismo punto como extremo. Por tanto, la multiplicidad de Punto a Linea sería 0..* (cero o muchas líneas). Esto significa que un punto no está obligado a formar parte de una línea, pero podría participar en muchas de ellas.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

En orientación a objetos, la composición fuerte y la composición débil se diferencian principalmente en el grado de dependencia que existe entre los objetos que participan en la relación. Ambas describen relaciones del tipo “A tiene un B”, pero no implican el mismo nivel de control sobre los objetos contenidos. Esta distinción suele aparecer en el modelado de clases, especialmente en diagramas UML.

La composición fuerte implica una relación en la que el objeto contenido depende completamente del objeto que lo contiene. Es decir, el ciclo de vida del objeto parte está ligado al del objeto todo: cuando el objeto principal se crea, normalmente también se crean sus partes, y cuando el objeto principal se destruye, sus partes dejan de existir. En otras palabras, las partes no tienen sentido fuera del objeto que las contiene. A esta relación se le suele llamar simplemente composición.

En cambio, la composición débil describe una relación en la que los objetos pueden existir de forma independiente. El objeto que contiene a otro mantiene una referencia hacia él, pero no controla su ciclo de vida. El objeto contenido puede existir antes de la relación, seguir existiendo después o incluso ser compartido por varios objetos. A este tipo de relación se le suele llamar asociación o agregación.

En resumen, la diferencia clave está en la dependencia del ciclo de vida. En la composición fuerte, las partes dependen completamente del todo y se destruyen con él. En la composición débil, los objetos mantienen una relación estructural pero pueden existir de manera independiente. Por ello, en terminología habitual, se denomina composición a la relación fuerte y agregación o asociación a la relación débil.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza otra únicamente dentro de sus métodos —por ejemplo, al recibirla como parámetro, devolverla como resultado, crear una instancia con new dentro de un método o usarla como variable local— no se considera una relación de composición. En estos casos se habla de dependencia. La dependencia indica que una clase necesita usar temporalmente otra clase para realizar alguna operación, pero no la mantiene como parte de su estado interno.

La característica principal de la dependencia es que la relación es débil y puntual. La clase que usa a la otra no almacena referencias a ella como atributos, sino que simplemente la utiliza durante la ejecución de un método. Por ejemplo, un método podría recibir un objeto como parámetro para realizar un cálculo, o crear un objeto auxiliar dentro del método y descartarlo al finalizar. En estas situaciones, la relación entre ambas clases existe solo durante la ejecución del método.

En cambio, en la composición la relación es más fuerte y estructural, ya que una clase contiene objetos de otra como atributos. Esos objetos forman parte del estado del objeto principal y su relación suele mantenerse durante toda la vida del objeto. Por tanto, cuando una clase simplemente utiliza otra dentro de un método sin almacenarla como atributo, se considera una dependencia y no una composición.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

En el ejemplo de `Linea` y `Punto`, la relación puede programarse de dos maneras dependiendo de si el objeto que contiene controla o no el **ciclo de vida** de los objetos que utiliza. Cuando `Linea` crea y gestiona internamente los puntos, se habla de **composición fuerte**, ya que los puntos existen únicamente como parte de la línea. En cambio, si los puntos se crean fuera y se pasan a la línea, la relación es **composición débil (agregación)**, porque los puntos pueden existir independientemente.

En una **composición fuerte**, la clase `Linea` se encarga de crear los puntos dentro de su constructor. De esta forma, los objetos `Punto` no existen previamente ni pueden compartirse con otros objetos. El ciclo de vida de los puntos queda ligado al de la línea: cuando la línea deja de existir, también lo hacen sus puntos.

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}


public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

En una **composición débil (agregación)**, los puntos se crean fuera de la clase `Linea` y se pasan como parámetros al constructor. En este caso, la línea solo mantiene referencias a esos puntos, pero no controla su creación ni su existencia. Los mismos puntos podrían utilizarse en varias líneas o seguir existiendo aunque la línea desaparezca.


public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}


La diferencia esencial entre ambas implementaciones está en **quién crea los objetos `Punto`**. Si `Linea` los crea internamente, se trata de **composición fuerte**, porque controla su ciclo de vida. Si los puntos se crean fuera y se pasan a `Linea`, se trata de **composición débil o agregación**, ya que los objetos existen de forma independiente y pueden compartirse.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, en una relación de **composición fuerte**, no es necesario destruir explícitamente los objetos contenidos. A diferencia de lenguajes como C o C++, Java gestiona automáticamente la memoria mediante un mecanismo llamado **recolección de basura (garbage collection)**. Esto significa que los objetos se eliminan automáticamente cuando ya no existen referencias hacia ellos en el programa.

En el ejemplo de `Linea` y `Punto`, la clase `Linea` contiene referencias a dos objetos `Punto`. Mientras la instancia de `Linea` exista, también existirán referencias hacia esos puntos. Sin embargo, cuando el objeto `Linea` deja de ser accesible (por ejemplo, porque ya no hay ninguna variable que lo referencie), también desaparecen las únicas referencias que existían hacia los objetos `Punto`. En ese momento, dichos objetos quedan **inaccesibles**.

Cuando un objeto queda inaccesible, el recolector de basura de Java puede liberar la memoria que ocupaba en algún momento posterior. Por esta razón, no aparece ninguna llamada explícita para destruir los objetos `Punto`. El lenguaje se encarga automáticamente de detectar que ya no se utilizan y de eliminarlos cuando corresponda.

En consecuencia, en Java la composición fuerte no implica escribir código para destruir manualmente los objetos contenidos. La relación de ciclo de vida se establece de forma **lógica** (porque los puntos solo existen a través de la línea), mientras que la liberación real de memoria se realiza automáticamente mediante el sistema de gestión de memoria del lenguaje.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Este ejemplo representa una composición débil (agregación), ya que los objetos Profesor existen de forma independiente al Departamento. El departamento mantiene referencias a los profesores que pertenecen a él, pero no controla su creación ni su ciclo de vida. Además, se modelan dos relaciones simultáneamente: el departamento tiene varios profesores y tiene un director, que debe ser siempre uno de esos profesores.

Para mantener la invariante de clase (siempre debe existir un director y este debe pertenecer a la lista de profesores), el constructor exige un director inicial y lo añade automáticamente a la lista de profesores. Además, se lanzan excepciones cuando se intenta realizar una operación que violaría dicha condición, como eliminar al director o asignar como director a un profesor que no pertenezca al departamento. Internamente se utiliza un array Profesor[] de tamaño máximo 50, pero la clase no expone ese detalle y proporciona métodos controlados para acceder a los profesores.

public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director inicial no puede ser null");
        }

        profesores = new Profesor[50];
        profesores[0] = directorInicial;
        numProfesores = 1;
        director = directorInicial;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        return profesores[pos];
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor null");
        }
        if (numProfesores >= profesores.length) {
            throw new IllegalStateException("Departamento lleno");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("Director null");
        }

        boolean pertenece = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                pertenece = true;
                break;
            }
        }

        if (!pertenece) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }

        director = nuevoDirector;
    }
}

En este diseño se mantiene la encapsulación, ya que el array interno no se expone directamente. El acceso se realiza mediante métodos que permiten saber cuántos profesores hay y obtener uno por posición. Además, las operaciones de modificación verifican la invariante del sistema: siempre existe un director y este siempre pertenece al conjunto de profesores del departamento.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

En Java es habitual utilizar la interfaz List en lugar de arrays primitivos para gestionar colecciones de objetos. Una implementación común es ArrayList, que permite añadir y eliminar elementos dinámicamente sin preocuparse por el tamaño máximo ni por desplazar manualmente los elementos. Esto simplifica el código y mantiene el mismo modelo de composición débil entre Departamento y Profesor, donde el departamento solo mantiene referencias a los profesores.

import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director inicial no puede ser null");
        }

        profesores = new ArrayList<>();
        profesores.add(directorInicial);
        director = directorInicial;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor null");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(pos);
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}

Al utilizar List, se evita gran parte del código necesario cuando se empleaba un array. En particular, ya no es necesario mantener una variable numProfesores, comprobar manualmente si el array está lleno, ni desplazar los elementos al eliminar uno. Todas esas operaciones se delegan en los métodos de la colección (add, remove, size, get), lo que simplifica la implementación y reduce la probabilidad de errores.

Respecto a devolver todos los profesores a la vez, surgiría un problema si se devolviera directamente la lista interna. En ese caso, el código externo podría modificarla (por ejemplo, añadir o eliminar profesores), lo que rompería la encapsulación y podría violar las invariantes del objeto, como eliminar al director. Para evitarlo, se puede devolver una vista no modificable de la lista usando Collections.unmodifiableList(profesores), o bien devolver una copia de la lista. De esta forma, el código externo puede consultar los profesores, pero no modificar la estructura interna del departamento.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

En Java, una composición recursiva ocurre cuando una clase contiene referencias a objetos de su mismo tipo. Este patrón es habitual para modelar jerarquías o estructuras encadenadas, como árboles, listas enlazadas o relaciones familiares. La característica clave es que la clase se compone de instancias de sí misma, y la composición puede ser inmutable si se ocultan los atributos y no se proporcionan métodos que los modifiquen.

Un ejemplo sencillo es una clase Persona que tenga un nombre y una referencia a su madre. Para garantizar inmutabilidad, los atributos se declaran private final, se inicializan en el constructor y no existen métodos que permitan modificarlos después. La referencia a la madre puede ser null si no se conoce, por ejemplo en la primera generación.

public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
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

Un ejemplo de uso en un main creando varias generaciones sería:

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

En este ejemplo, la relación Persona -> madre -> Persona es recursiva, y la inmutabilidad garantiza que las relaciones no puedan cambiar tras la creación de cada objeto. Otros ejemplos clásicos de composiciones recursivas incluyen:

Árboles binarios o generales, donde cada nodo tiene referencias a sus hijos.

Listas enlazadas, donde cada nodo apunta al siguiente.

Excepciones encadenadas en Java (Throwable.getCause()), que permite rastrear la causa original de un error.

Estructuras de carpetas y archivos en sistemas de ficheros, donde un directorio contiene subdirectorios.

Estas composiciones recursivas permiten modelar relaciones jerárquicas de manera natural y aprovechar la reutilización de la misma clase para representar distintos niveles de la estructura.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales ocurren cuando dos clases mantienen referencias mutuas: cada objeto de la primera clase conoce a los objetos de la segunda clase, y cada objeto de la segunda clase sabe a qué objeto de la primera clase pertenece. Esto permite navegar la relación en ambas direcciones, pero introduce complejidad adicional, ya que se deben mantener consistentes ambas referencias cada vez que se añade, elimina o cambia un objeto de la relación.

En el ejemplo de Profesor y Departamento, actualmente la relación es unidireccional: Departamento conoce a sus profesores, pero un Profesor no sabe a qué departamento pertenece. Para convertirla en bidireccional, cada objeto Profesor debería tener un atributo que haga referencia a su Departamento. Al añadir un profesor a un departamento, además de incluirlo en la lista interna del departamento, se debería establecer su referencia interna al departamento correspondiente. De forma análoga, al eliminarlo del departamento, la referencia del profesor a ese departamento debería actualizarse a null. Esto garantiza que ambos lados de la relación permanezcan consistentes.

Un ejemplo conceptual sería:

public class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia al departamento

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }

    public Departamento getDepartamento() { return departamento; }

    void setDepartamento(Departamento depto) { // package-private para control
        this.departamento = depto;
    }
}

public class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores = new ArrayList<>();
        profesores.add(directorInicial);
        director = directorInicial;
        directorInicial.setDepartamento(this); // actualizar referencia bidireccional
    }

    public void añadirProfesor(Profesor p) {
        profesores.add(p);
        p.setDepartamento(this); // mantener referencia bidireccional
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
        p.setDepartamento(null); // actualizar referencia al eliminar
    }
}

En este diseño, la relación es bidireccional: el departamento conoce a sus profesores y cada profesor sabe a qué departamento pertenece. Este tipo de implementación requiere cuidar las invariantes y encapsular los métodos de actualización para evitar inconsistencias o referencias “huérfanas”. Además, conviene que la modificación de la referencia del lado del profesor sea controlada únicamente por la clase Departamento para garantizar coherencia.