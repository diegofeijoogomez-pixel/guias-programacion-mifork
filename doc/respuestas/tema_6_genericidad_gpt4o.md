# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En lenguajes como C, cuando no se dispone de genericidad, es habitual recurrir a punteros genéricos (void*) para construir estructuras de datos capaces de almacenar elementos de cualquier tipo. La idea consiste en utilizar un array de punteros sin tipo concreto, de manera que cada posición pueda apuntar a datos de distinta naturaleza. Sin embargo, esta flexibilidad implica perder comprobación de tipos en tiempo de compilación, por lo que es responsabilidad del programador realizar las conversiones adecuadas (cast) al recuperar los datos.

Un ejemplo sencillo sería una estructura tipo lista dinámica que almacena punteros genéricos. Internamente se mantiene un array de void*, y se insertan direcciones de variables de distintos tipos. Posteriormente, al acceder a los elementos, es necesario conocer el tipo original para interpretar correctamente el contenido. Este enfoque es potente, pero propenso a errores si se realizan conversiones incorrectas o se gestionan mal las direcciones de memoria.

#include <stdio.h>

#define MAX 10

typedef struct {
    void* data[MAX];
    int size;
} GenericArray;

void add(GenericArray* arr, void* element) {
    if (arr->size < MAX) {
        arr->data[arr->size++] = element;
    }
}

int main() {
    GenericArray arr = { .size = 0 };

    int a = 10;
    float b = 3.14;
    char c = 'x';

    add(&arr, &a);
    add(&arr, &b);
    add(&arr, &c);

    // Recuperación con casting
    printf("%d\n", *(int*)arr.data[0]);
    printf("%f\n", *(float*)arr.data[1]);
    printf("%c\n", *(char*)arr.data[2]);

    return 0;
}

En Java ocurre algo similar si se utiliza Object como tipo base, ya que todas las clases heredan de él. Se puede construir una estructura que internamente almacene un array de Object, permitiendo guardar cualquier tipo de objeto. No obstante, al recuperar los elementos es necesario realizar un casting explícito al tipo original, lo que también elimina parte de la seguridad de tipos en tiempo de compilación.

class GenericArray {
    private Object[] data;
    private int size;

    public GenericArray(int capacity) {
        data = new Object[capacity];
        size = 0;
    }

    public void add(Object element) {
        data[size++] = element;
    }

    public Object get(int index) {
        return data[index];
    }
}

public class Main {
    public static void main(String[] args) {
        GenericArray arr = new GenericArray(10);

        arr.add(10);        // Integer (autoboxing)
        arr.add(3.14);      // Double
        arr.add("hola");    // String

        int a = (Integer) arr.get(0);
        double b = (Double) arr.get(1);
        String c = (String) arr.get(2);

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
    }
}


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos con los que van a trabajar. En lugar de escribir múltiples versiones de una misma estructura (por ejemplo, una lista para enteros, otra para cadenas, etc.), se define una única implementación parametrizada por tipos. De este modo, el compilador puede garantizar la seguridad de tipos en tiempo de compilación, evitando errores derivados de conversiones incorrectas.

En lenguajes modernos como Java, esto se materializa mediante el uso de parámetros de tipo (por ejemplo, <T>), lo que permite escribir código reutilizable y seguro. A diferencia del uso de Object o void*, la genericidad mantiene la información de tipo durante la compilación, eliminando la necesidad de realizar conversiones explícitas y reduciendo el riesgo de errores en tiempo de ejecución. Por tanto, se mejora tanto la reutilización del código como su robustez.

El ejemplo anterior, basado en void* en C o Object en Java, no constituye un ejemplo de programación genérica en sentido estricto. Aunque permite almacenar datos de distintos tipos, lo hace a costa de perder la comprobación de tipos en tiempo de compilación. Este enfoque se considera una solución previa o alternativa a la genericidad, pero no cumple con sus principios fundamentales, especialmente en lo relativo a la seguridad de tipos.

En consecuencia, dicho ejemplo puede entenderse como una aproximación rudimentaria a la idea de abstracción de tipos, pero no como una implementación real de programación genérica. La verdadera genericidad implica que el compilador conozca y controle los tipos utilizados, algo que no ocurre en el uso de void* ni de Object sin parámetros genéricos.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema de utilizar void* en C o Object en Java para simular estructuras de datos genéricas es la pérdida de comprobación de tipos en tiempo de compilación. Al almacenar todos los elementos como punteros genéricos o como instancias de la clase base, el compilador deja de verificar si los tipos utilizados son correctos. Esto implica que se pueden insertar y recuperar datos de manera inconsistente sin que se detecte ningún error hasta la ejecución del programa.

Como consecuencia directa, aparece la necesidad de realizar conversiones explícitas (casting) al recuperar los elementos. Estas conversiones dependen completamente del programador, quien debe recordar el tipo real de cada dato almacenado. Si se realiza un casting incorrecto, en C se pueden obtener resultados impredecibles o errores graves (como corrupción de memoria), mientras que en Java se producirá una excepción en tiempo de ejecución (por ejemplo, ClassCastException).

Otro problema relevante es que se pierde la expresividad del código y su capacidad de autocomprobación. Una estructura de datos genérica basada en void* o Object no indica qué tipos de datos se esperan realmente, lo que dificulta su uso correcto por parte de otros programadores o incluso del propio desarrollador en fases posteriores. Esto incrementa la probabilidad de errores y reduce la mantenibilidad del código.

Por último, este enfoque impide que el compilador detecte errores de tipo de forma temprana, trasladando los fallos al tiempo de ejecución. Esto no solo complica la depuración, sino que también reduce la fiabilidad del software, ya que errores que podrían haberse evitado en compilación solo se manifiestan cuando el programa ya está en funcionamiento.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son un mecanismo que permite definir clases, interfaces o métodos de forma abstracta respecto al tipo de datos con el que operan. En lugar de utilizar un tipo concreto (como int, String o Object), se emplea un identificador simbólico —habitualmente una letra como T— que representa un tipo que será especificado posteriormente. De este modo, se consigue escribir código reutilizable que funciona con distintos tipos sin necesidad de duplicar implementaciones.

Este mecanismo es la base de la programación genérica en lenguajes como Java. Cuando se declara una clase con parámetros de tipo, se indica que el tipo concreto será proporcionado en el momento de crear instancias de dicha clase. El compilador utiliza esa información para comprobar que todas las operaciones realizadas son coherentes con el tipo especificado, manteniendo así la seguridad de tipos en tiempo de compilación.

Por ejemplo, se puede definir una estructura de datos genérica como una lista que almacena elementos de tipo T. Posteriormente, al crear un objeto de esa clase, se sustituye T por un tipo concreto (como Integer o String). A diferencia del uso de Object, no es necesario realizar conversiones explícitas al recuperar los elementos, ya que el compilador conoce el tipo exacto en cada caso.

class Caja<T> {
    private T contenido;

    public void guardar(T valor) {
        contenido = valor;
    }

    public T obtener() {
        return contenido;
    }
}

// Uso
Caja<Integer> caja = new Caja<>();
caja.guardar(10);
int valor = caja.obtener();  // Sin casting

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En Java, la programación genérica se implementa mediante generics, lo que permite definir estructuras de datos parametrizadas por tipo. Un ejemplo típico es el uso de ArrayList, que puede restringirse a un tipo concreto como String. Al especificar el tipo en la declaración, el compilador garantiza que solo se puedan insertar elementos de ese tipo, evitando errores de mezcla de tipos en tiempo de compilación.

Al recorrer la estructura, no es necesario realizar conversiones explícitas, ya que el sistema de tipos mantiene la información del tipo genérico. Esto permite trabajar directamente con objetos String, asegurando tanto la seguridad de tipos como la legibilidad del código.

import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println(s + " -> tipo: " + s.getClass().getSimpleName());
        }
    }
}

En C++, la programación genérica se implementa mediante templates, que permiten definir estructuras y clases parametrizadas por tipo en tiempo de compilación. Un vector de la STL es un ejemplo de estructura genérica que puede restringirse a un tipo concreto como string. Al igual que en Java, esto permite garantizar que solo se almacenen elementos del tipo especificado.

Durante el recorrido del vector, cada elemento ya es tratado directamente como string, sin necesidad de conversiones, lo que mantiene la seguridad de tipos y mejora la eficiencia, ya que la resolución del tipo se realiza en compilación.

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("Templates");

    for (const std::string& s : lista) {
        std::cout << s << " -> tipo: string" << std::endl;
    }

    return 0;
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se instancia una clase con parámetros de tipo, el compilador debe “concretar” esos tipos genéricos para poder garantizar la seguridad de tipos. En ese momento, se comprueba que las operaciones realizadas con los tipos son válidas y se adapta el código según el mecanismo que utilice el lenguaje. Aunque el objetivo en Java y C++ es similar (reutilización con tipos seguros), el funcionamiento interno es distinto.

En Java, el compilador realiza un proceso llamado type erasure (borrado de tipos). Esto significa que la información de los parámetros genéricos se utiliza únicamente en tiempo de compilación para comprobar errores, pero luego se elimina en el bytecode. En ejecución, todos los tipos genéricos se sustituyen por su tipo base (normalmente Object o el tipo límite si existe). Por ello, no existe información real del tipo genérico en tiempo de ejecución, y las conversiones necesarias se insertan automáticamente por el compilador.

En cambio, en C++, la programación genérica mediante templates funciona de forma distinta. El compilador realiza una instanciación de plantillas, lo que implica generar una versión específica del código para cada tipo concreto utilizado. Es decir, si se usa vector<string>, el compilador genera una implementación específica para string, y si se usa vector<int>, genera otra distinta. Esto ocurre en tiempo de compilación, produciendo código ya especializado para cada tipo.

Por tanto, no se comportan igual: Java reutiliza una única estructura mediante borrado de tipos, mientras que C++ genera múltiples versiones del código para cada tipo utilizado. Esto hace que C++ tenga más especialización y potencial optimización en tiempo de ejecución, mientras que Java simplifica la máquina virtual pero pierde información de tipo en tiempo de ejecución.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Se puede definir una clase genérica Par utilizando dos parámetros de tipo, de manera que cada uno de ellos pueda ser distinto. Esto permite almacenar dos valores relacionados sin perder la seguridad de tipos. Cada parámetro de tipo se mantiene durante la compilación, evitando la necesidad de conversiones explícitas al recuperar los datos.

La clase incluye un constructor para inicializar ambos valores y métodos getter para acceder a cada uno de ellos. Al ser una clase genérica, el compilador garantiza que los tipos utilizados sean consistentes en el momento de su instanciación.

class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

Un uso típico de esta clase es devolver múltiples valores desde un método. Por ejemplo, al calcular estadísticas sobre un array de double, se puede devolver la media y la desviación típica en un único objeto Par<Double, Double>. De este modo, se evita crear clases específicas adicionales solo para agrupar resultados.

El método que realiza el cálculo encapsula ambos resultados en un Par, y al recuperarlos no es necesario realizar casting, ya que los tipos están definidos explícitamente en la instanciación del objeto.

public class Estadisticas {

    public static Par<Double, Double> calcular(double[] datos) {
        double suma = 0;

        for (double d : datos) {
            suma += d;
        }

        double media = suma / datos.length;

        double varianza = 0;
        for (double d : datos) {
            varianza += Math.pow(d - media, 2);
        }

        double desviacion = Math.sqrt(varianza / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] datos = {1.0, 2.0, 3.0, 4.0, 5.0};

        Par<Double, Double> resultado = calcular(datos);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

En Java, además de definir clases genéricas, también es posible declarar métodos genéricos con sus propios parámetros de tipo. Esto permite que la inferencia de tipos se realice únicamente a nivel del método, sin necesidad de que toda la clase sea genérica. En este caso, el método seleccionaUno puede definirse con un parámetro de tipo T, de forma que ambos argumentos y el valor de retorno sean del mismo tipo.

El uso de parámetros de tipo en el método permite que el compilador garantice que los dos objetos pasados son del mismo tipo, evitando errores en tiempo de compilación. Además, elimina la necesidad de realizar downcasting al recuperar el resultado, ya que el tipo de retorno se conoce de forma estática.

import java.util.Random;

public class Util {

    public static <T> T seleccionaUno(T a, T b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String resultado = seleccionaUno("Hola", "Mundo");
        System.out.println(resultado);
    }
}

En cambio, si se define el mismo método utilizando Object, ambos parámetros se tratan como tipos genéricos sin control. Esto permite pasar objetos de tipos diferentes sin que el compilador lo detecte como error. Además, el valor devuelto será de tipo Object, por lo que será necesario realizar un casting explícito al tipo deseado, introduciendo riesgo de errores en tiempo de ejecución.

import java.util.Random;

public class UtilObject {

    public static Object seleccionaUno(Object a, Object b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        Object resultado = seleccionaUno("Hola", 123);

        String texto = (String) resultado; // posible ClassCastException
        System.out.println(texto);
    }
}

En términos de diferencias, el uso de parámetros de tipo evita el downcasting, ya que el tipo de retorno se conoce en compilación. Además, obliga a que ambos parámetros sean del mismo tipo concreto, impidiendo combinaciones inconsistentes como String e Integer. En cambio, la versión con Object pierde toda esta información, trasladando posibles errores a tiempo de ejecución y reduciendo la seguridad del código.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer restricciones (bounds) sobre los parámetros de tipo. Esto permite indicar que un tipo genérico debe cumplir una condición, como por ejemplo pertenecer a una jerarquía concreta. En el caso de los números, se puede restringir T para que sea al menos un Number, utilizando la sintaxis T extends Number. Esto permite tratar el valor como número dentro de la clase, aunque manteniendo la genericidad.

Una primera solución (menos segura) consiste en definir directamente las coordenadas como Number. Esto permite almacenar cualquier subtipo (Integer, Double, etc.), pero obliga a realizar conversiones explícitas al operar, ya que el tipo concreto se pierde.

class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En esta versión, aunque se admite cualquier tipo de Number, no se fuerza que ambos puntos utilicen el mismo subtipo concreto (por ejemplo, un Punto puede mezclar Integer y Double), lo que reduce la seguridad de tipos.

Una segunda solución utiliza genéricos con restricción, asegurando que el tipo utilizado sea numérico y consistente en todo el objeto. Esto mejora el chequeo en compilación y permite trabajar de forma más segura con el tipo concreto.

class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En esta versión, el compilador garantiza que ambos puntos usan el mismo tipo numérico (T), evitando inconsistencias como mezclar Integer y Double en operaciones entre objetos.

Respecto al type erasure, en ambos casos el tipo genérico desaparece en tiempo de compilación. En la versión con Punto<T>, el tipo final tras la compilación es simplemente Punto, y todas las referencias a T se sustituyen por su límite superior (Number). Por tanto, en ejecución no existe información sobre T, aunque el compilador haya verificado su corrección previamente.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

En la primera solución, en la que se utilizan directamente atributos de tipo Number, se permite almacenar en las coordenadas cualquier subtipo de Number de forma independiente. Esto significa que es posible crear un punto donde una coordenada sea un Integer y la otra un Double, ya que ambas son tratadas únicamente como Number. Sin embargo, esta flexibilidad implica una pérdida de control sobre la coherencia del tipo concreto utilizado en cada coordenada.

En esta misma solución sin genéricos, el método getX devuelve siempre un tipo Number, independientemente de si el valor real almacenado es un Integer, Double u otro subtipo. Esto obliga al programador a realizar conversiones explícitas si se necesita trabajar con un tipo más específico, lo que puede introducir errores en tiempo de ejecución si no se realiza correctamente el casting.

En la solución con genéricos, donde se define Punto<T extends Number>, no se permite mezclar tipos distintos en las coordenadas del mismo objeto. Es decir, no se puede crear un punto con una coordenada Integer y otra Double, ya que ambas deben ser del mismo tipo T. Esto refuerza el chequeo de tipos y asegura consistencia interna en el objeto.

En esta versión genérica, el método getX devuelve directamente el tipo T, que será el tipo concreto elegido al instanciar el punto (por ejemplo, Integer o Double). Esto elimina la necesidad de conversiones explícitas y permite trabajar con un tipo más preciso en tiempo de compilación. En ambos casos, debido al type erasure, en tiempo de ejecución el tipo genérico desaparece, pero en la versión con genéricos se mantiene la seguridad de tipos en compilación, que es donde realmente se detectan los errores.

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

El problema del código original es que la interfaz Punto es demasiado genérica respecto al tipo del propio punto. Al no parametrizar el tipo, el método distanciaA(Punto p) obliga a aceptar cualquier implementación de Punto, lo que fuerza a usar instanceof y downcasting para comprobar el tipo real en tiempo de ejecución. Esto rompe la seguridad de tipos y traslada errores al runtime.

La solución mediante generics con autolimitación (self-referential types) consiste en parametrizar la interfaz con el propio tipo de punto. Es decir, se introduce un parámetro T que representa el tipo concreto de punto con el que se puede interactuar. De este modo, se garantiza que un Punto2D solo pueda calcular distancia con otro Punto2D, y lo mismo para Punto3D, eliminando completamente la necesidad de comprobaciones dinámicas.

public interface Punto<T> {
    double distanciaA(T p);
}

En las implementaciones, se fija el parámetro de tipo al propio tipo de clase. Esto asegura que el compilador controle la coherencia entre los objetos usados en el método distanciaA, evitando mezclas entre puntos de distinta dimensión.

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2)
                + Math.pow(z - p.z, 2));
    }
}

Con esta solución, el compilador impide de forma estática comparar un Punto2D con un Punto3D, ya que los tipos no son compatibles (Punto2D no puede recibir Punto3D como parámetro). Esto elimina por completo la necesidad de instanceof y downcasting, ya que el tipo correcto se garantiza en compilación.

Además, el uso de generics mejora la expresividad del diseño: el contrato de la interfaz indica explícitamente con qué tipo de punto se trabaja, y cualquier inconsistencia se detecta antes de ejecutar el programa.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Que String sea subtipo de Object no implica automáticamente que List<String> sea subtipo de List<Object>. En Java, los tipos genéricos como List<T> son invariantes, lo que significa que aunque String sí es subtipo de Object, no existe relación de subtipado entre List<String> y List<Object>. Esto se hace intencionalmente para evitar errores de tipo en tiempo de compilación. Si esa relación existiera, se podría introducir un Integer dentro de una lista de String a través de una referencia más genérica, rompiendo la seguridad de tipos.

En cambio, los arrays en Java sí son covariantes, lo que significa que String[] es considerado subtipo de Object[]. Esto permite asignar un array de String a una referencia de Object[]. Sin embargo, esta flexibilidad introduce un problema en tiempo de ejecución: el error no se detecta en compilación, sino cuando se intenta insertar un elemento incompatible.

Object[] arr = new String[2];
arr[0] = "hola";
arr[1] = 123; // Error en tiempo de ejecución: ArrayStoreException

En este caso, el compilador permite la asignación porque String[] es subtipo de Object[], pero en ejecución la JVM detecta que el array real es de String y lanza una excepción (ArrayStoreException) al intentar insertar un Integer. Esto demuestra que la covarianza en arrays puede comprometer la seguridad de tipos en tiempo de ejecución.

A partir de estos ejemplos, se definen tres conceptos clave respecto a los genéricos:

Un tipo es covariante si permite que la relación de subtipado se conserve en la misma dirección. Es decir, si A es subtipo de B, entonces F<A> es subtipo de F<B>. Esto ocurre con arrays en Java, pero no con genéricos estándar.
Un tipo es contravariante si invierte la relación de subtipado. Es decir, si A es subtipo de B, entonces F<B> es subtipo de F<A>. Esto se utiliza en Java con comodines del tipo ? super T, principalmente en escenarios de escritura.
Un tipo es invariante si no existe ninguna relación de subtipado entre F<A> y F<B> aunque A y B sí estén relacionados. Esto ocurre con los genéricos en Java (List<String> no es subtipo de List<Object>), lo que mejora la seguridad de tipos en compilación y evita errores como los de los arrays.

En resumen, Java prioriza la seguridad en los genéricos mediante invariancia, mientras que los arrays mantienen covarianza por razones históricas, lo que puede provocar errores en tiempo de ejecución si no se usan con cuidado.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard (?) en Java es un comodín que se utiliza en programación genérica para representar un tipo desconocido dentro de una jerarquía de tipos. Permite flexibilizar el uso de genéricos sin perder completamente la seguridad de tipos, controlando si se permite leer, escribir o ambas operaciones sobre una estructura genérica.

La diferencia entre List<? extends T> y List<? super T> está en la dirección de la jerarquía de tipos que se permite:

List<? extends T> indica que la lista contiene elementos de un tipo desconocido que es T o un subtipo de T. Se utiliza cuando interesa leer datos de la estructura, pero no modificarla (excepto null), ya que no se puede garantizar el tipo exacto de inserción.
List<? super T> indica que la lista contiene elementos de un tipo desconocido que es T o un supertipo de T. Se utiliza cuando interesa escribir datos en la estructura, ya que se garantiza que cualquier T es válido para insertarse, aunque al leer solo se obtiene seguridad como Object.

Un primer ejemplo consiste en un método que calcula la suma de una lista de números. En este caso se utiliza ? extends Number, ya que solo interesa leer valores numéricos, no modificarlos. Esto permite aceptar listas de Integer, Double, etc., manteniendo la flexibilidad.

import java.util.List;

public class Utils {

    public static double suma(List<? extends Number> lista) {
        double suma = 0;

        for (Number n : lista) {
            suma += n.doubleValue();
        }

        return suma;
    }
}

En este caso, el uso de extends garantiza que todos los elementos pueden tratarse como Number, pero impide añadir nuevos elementos a la lista, ya que el tipo concreto podría ser más específico (por ejemplo, List<Integer>).

El segundo ejemplo muestra un método que añade valores enteros a una lista. Aquí se utiliza ? super Integer, ya que interesa garantizar que la lista pueda aceptar enteros o sus supertipos (como Number u Object). Esto permite escritura segura.

import java.util.List;

public class UtilsAdd {

    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
        lista.add(30);
    }
}

En este caso, aunque no se conoce el tipo exacto de la lista al leer, se garantiza que cualquier valor Integer puede insertarse sin riesgo de incompatibilidad.

En resumen, ? extends se utiliza para consumir (leer) datos de forma segura (covarianza controlada), mientras que ? super se utiliza para producir (escribir) datos de forma segura (contravarianza controlada). Esto sigue la regla general “PECS” (Producer Extends, Consumer Super).