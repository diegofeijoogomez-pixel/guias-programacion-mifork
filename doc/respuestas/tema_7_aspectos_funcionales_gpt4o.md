# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función es una variable que almacena la dirección de memoria de una función, de forma similar a cómo un puntero normal almacena la dirección de una variable. Esto permite tratar a las funciones como datos: se pueden pasar como argumentos, almacenarlas en estructuras o invocarlas indirectamente. En C, esta característica es habitual y resulta clave para implementar comportamientos flexibles, como callbacks o estrategias intercambiables, algo que en Java se suele lograr mediante interfaces o clases funcionales.

La sintaxis de un puntero a función puede parecer compleja al principio, ya que debe reflejar exactamente la firma de la función (tipo de retorno y parámetros). Es importante notar que el nombre de la función ya actúa como un puntero a su dirección, por lo que puede asignarse directamente a una variable puntero. Posteriormente, dicha variable puede utilizarse para invocar la función como si se tratase de una llamada normal.

A continuación se muestra un ejemplo donde se define una función que recibe una cadena de caracteres y la convierte a mayúsculas. Después, se crea un puntero a dicha función llamado aMayusculas y se utiliza para invocarla:

#include <stdio.h>
#include <ctype.h>

void convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "hola mundo";

    // Definición del puntero a función
    void (*aMayusculas)(char *) = convertirAMayusculas;

    // Invocación mediante el puntero
    aMayusculas(texto);

    printf("%s\n", texto);

    return 0;
}

En este ejemplo, la función modifica la cadena original (comportamiento típico en C al trabajar con arrays de caracteres). El puntero aMayusculas permite desacoplar la llamada concreta de la función, lo que facilita reutilización y extensibilidad. Este mecanismo es conceptualmente similar a pasar métodos como referencia en paradigmas más modernos, aunque en C se realiza de forma explícita mediante punteros.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima (sin nombre) que puede definirse de forma inline y tratarse como un valor. Esto permite asignarla a variables, pasarla como argumento o devolverla desde otras funciones. En comparación con C, donde se utilizan punteros a funciones, las lambdas ofrecen una sintaxis más compacta y segura, además de integrarse mejor con abstracciones de alto nivel. En Java, este concepto se introdujo a partir de Java 8 como parte del soporte para programación funcional, mientras que en JavaScript ha sido una característica fundamental desde sus inicios.

Las funciones lambda suelen utilizarse para expresar comportamientos pequeños y específicos sin necesidad de declarar una función completa. Esto resulta especialmente útil en operaciones sobre colecciones, callbacks o transformaciones de datos. Conceptualmente, se puede ver como una evolución del uso de punteros a función en C, pero con mejor legibilidad y tipado más expresivo (en el caso de Java).

A continuación se muestra un ejemplo en JavaScript, donde se define una función lambda que convierte una cadena a mayúsculas y se asigna a una variable local aMayusculas:

const aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let texto = "hola mundo";
console.log(aMayusculas(texto));

En Java, se puede lograr algo equivalente utilizando interfaces funcionales como Function<String, String>, que representan funciones que reciben un valor y devuelven otro. La lambda se asigna a la variable aMayusculas y posteriormente se invoca mediante el método apply:

import java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String texto = "hola mundo";
        System.out.println(aMayusculas.apply(texto));
    }
}

En ambos casos, la función lambda encapsula un comportamiento sencillo sin necesidad de crear una función con nombre. En Java, esto sustituye en muchos casos a clases anónimas, mientras que en JavaScript es una forma habitual y concisa de definir funciones.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación en el que el eje principal son las funciones puras y la evaluación de expresiones, en lugar de la modificación de estados o variables. Se basa en la idea de que una función, dado un mismo conjunto de entradas, siempre producirá la misma salida y no generará efectos secundarios (como modificar variables globales o estructuras externas). Esto contrasta con la programación imperativa tradicional (como en C), donde se describe paso a paso cómo cambiar el estado del programa. En el paradigma funcional se prioriza el “qué” se quiere obtener, no el “cómo” hacerlo paso a paso.

Un lenguaje como Java 8 se considera multi-paradigma porque combina características de distintos estilos de programación. Aunque originalmente Java es un lenguaje orientado a objetos (todo gira en torno a clases y objetos), a partir de Java 8 se incorporaron elementos funcionales como funciones lambda, streams y referencias a métodos. Esto permite elegir el enfoque más adecuado según el problema: se puede seguir utilizando herencia y polimorfismo, o bien emplear funciones y transformaciones sobre datos de forma declarativa, más cercana al paradigma funcional.

Decir que las funciones son “ciudadanos de primera clase” significa que pueden tratarse igual que cualquier otro valor del lenguaje. Es decir, pueden almacenarse en variables, pasarse como argumentos a otras funciones, devolverse como resultado y componerse entre sí. En C esto se logra parcialmente mediante punteros a funciones, mientras que en lenguajes más modernos como Java o JavaScript se consigue de forma más directa y segura mediante lambdas y tipos funcionales. Esta capacidad es fundamental en el paradigma funcional, ya que permite construir abstracciones más flexibles y reutilizables.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis básica de una función lambda en Java se introdujo en Java 8 como una forma compacta de representar implementaciones de interfaces funcionales (interfaces con un único método abstracto). Una lambda se compone de tres partes: la lista de parámetros, el operador flecha -> y el cuerpo de la función. De forma general, su estructura es: (parámetros) -> { cuerpo }. Esta sintaxis permite definir comportamientos de forma inline sin necesidad de crear clases anónimas, lo que simplifica considerablemente el código.

En cuanto a los parámetros, pueden declararse con o sin tipo, ya que el compilador suele inferirlo automáticamente. Si hay un solo parámetro, incluso se pueden omitir los paréntesis. El cuerpo puede ser una expresión simple o un bloque de código. Si es una única expresión, no es necesario usar llaves {} ni la palabra clave return, ya que el valor se devuelve implícitamente. En cambio, si el cuerpo contiene varias instrucciones, sí deben utilizarse llaves y return cuando corresponda.

Por ejemplo, una lambda sencilla que recibe una cadena y devuelve su versión en mayúsculas podría escribirse así:

Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

Si se quisiera una versión más explícita con bloque de código, sería equivalente a:

Function<String, String> aMayusculas = (String cadena) -> {
    return cadena.toUpperCase();
};

Esta sintaxis se integra con el sistema de tipos de Java a través de interfaces funcionales como Function, Consumer o Predicate. De este modo, aunque Java sigue siendo orientado a objetos, las lambdas permiten trabajar con funciones de forma más directa y expresiva, acercándose al estilo del paradigma funcional.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

La idea consiste en tratar la función como un valor que se puede pasar como argumento a otro método, algo característico del enfoque funcional. En lugar de fijar el comportamiento dentro del propio método, este se parametriza mediante una función externa, lo que permite reutilizar el mismo método con distintas transformaciones. Esto es conceptualmente similar a pasar punteros a función en C, pero con una sintaxis más clara y segura en lenguajes modernos como Java y JavaScript.

En JavaScript, las funciones son ciudadanos de primera clase, por lo que se pueden pasar directamente como argumentos. El método transformar recibe una cadena y una función transformadora, y simplemente aplica dicha función sobre el texto recibido:

const aMayusculas = (cadena) => cadena.toUpperCase();

function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

let resultado = transformar("hola mundo", aMayusculas);
console.log(resultado);

En Java, esto se consigue utilizando interfaces funcionales como Function<String, String>. El método transformar recibe tanto la cadena como la función, y la invoca mediante el método apply:

import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}

En ambos casos, el método transformar queda desacoplado de la lógica concreta de transformación. Esto permite aplicar diferentes funciones (por ejemplo, convertir a minúsculas, invertir la cadena, etc.) sin modificar su implementación, favoreciendo así la reutilización y la flexibilidad del código.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

En este caso se da un paso más en el uso de funciones como valores: en lugar de definir previamente la función en una variable (como aMayusculas), se define directamente en el momento de la llamada. Esto es habitual en programación funcional cuando la función es simple o se va a utilizar una única vez. De esta forma, se reduce la necesidad de nombres auxiliares y se hace el código más conciso.

En JavaScript, esto resulta especialmente natural, ya que las funciones lambda (arrow functions) forman parte del uso cotidiano del lenguaje. La función que invierte la cadena se define inline y se pasa directamente como argumento a transformar:

function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

let resultado = transformar("hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);

En Java, el enfoque es similar, aunque se requiere el uso de una interfaz funcional como Function<String, String>. La lambda se define directamente en la llamada al método transformar, manteniendo la misma idea de inmediatez:

import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("hola mundo", (cadena) -> {
            return new StringBuilder(cadena).reverse().toString();
        });

        System.out.println(resultado);
    }
}

Este enfoque permite expresar comportamientos puntuales de forma directa y localizada, sin necesidad de crear variables adicionales. Es especialmente útil en contextos donde las funciones son pequeñas y su uso es inmediato, lo que mejora la legibilidad siempre que no se abuse de lambdas excesivamente complejas.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre (closure) es una función que “recuerda” el entorno en el que fue creada, pudiendo acceder a variables externas a su propio ámbito incluso después de que dicho ámbito haya finalizado. En el contexto de las funciones lambda, esto significa que la lambda puede capturar variables locales del entorno donde se define. A diferencia de C, donde esto no es automático, lenguajes como Java o JavaScript permiten este comportamiento de forma controlada, facilitando la creación de funciones más expresivas y flexibles.

En Java, las lambdas pueden acceder a variables locales del contexto que las rodea, pero con una restricción importante: dichas variables deben ser efectivamente finales (es decir, no deben modificarse después de su inicialización). Esto garantiza seguridad y evita problemas relacionados con concurrencia o estados inconsistentes. Este mecanismo permite que la lambda utilice valores externos sin necesidad de pasarlos explícitamente como parámetros.

A continuación se muestra una modificación del ejemplo anterior. Se define una variable local adicional que contiene una cadena a concatenar, y se crea una lambda que utiliza dicha variable capturada:

import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = "!!!"; // variable local capturada (closure)

        String resultado = transformar("hola mundo", (cadena) -> {
            return cadena + sufijo;
        });

        System.out.println(resultado);
    }
}

En este ejemplo, la lambda accede a la variable sufijo definida fuera de ella, lo que demuestra el concepto de cierre. Aunque sufijo no es un parámetro de la función, su valor queda “capturado” en el momento de la definición de la lambda. Este comportamiento permite construir funciones más dinámicas y reutilizables, acercando Java a los principios del paradigma funcional.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La diferencia principal radica en el nivel de abstracción y en las capacidades asociadas. Un puntero a función en C es simplemente una dirección de memoria que apunta a código ejecutable. Permite invocar funciones de forma indirecta, pero no tiene conocimiento del contexto en el que fue definido ni puede capturar variables externas. En cambio, una función lambda es una construcción de más alto nivel que no solo representa comportamiento, sino que también puede llevar asociado un entorno (closure), lo que le permite acceder a variables externas de forma segura.

Otra diferencia importante es el tipado y la integración con el lenguaje. En C, los punteros a función requieren una sintaxis explícita y a menudo compleja, además de ser menos seguros (no hay verificación avanzada más allá de la firma). En Java, las lambdas están fuertemente tipadas mediante interfaces funcionales, lo que facilita su uso y reduce errores. Además, su sintaxis es más concisa y legible, especialmente cuando se utilizan para operaciones pequeñas o como argumentos de métodos.

También se diferencian en la forma de trabajar con el estado. Los punteros a función no pueden capturar variables locales automáticamente; cualquier dato adicional debe pasarse explícitamente como parámetro. Por el contrario, las lambdas pueden capturar variables del entorno (si son efectivamente finales en Java), lo que permite construir funciones más expresivas sin necesidad de ampliar su lista de parámetros. Esto es clave en el paradigma funcional, donde las funciones no solo ejecutan código, sino que también encapsulan parte del contexto en el que se definen.

En conjunto, se puede considerar que las lambdas son una evolución conceptual de los punteros a función: ofrecen la misma idea básica de tratar funciones como valores, pero con mayor seguridad, expresividad y capacidad de integración con paradigmas modernos como el funcional.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

En este caso se trabaja con una idea clave del paradigma funcional: las funciones que devuelven otras funciones. La función crearDescuento no aplica directamente el descuento, sino que genera y devuelve una función configurada con un porcentaje concreto. Esto permite reutilizar lógica creando “funciones especializadas”, algo que en programación orientada a objetos suele resolverse con clases o estrategias, pero que aquí se consigue de forma más ligera mediante lambdas.

En Java, esto se implementa utilizando Function<Double, Double>. La función externa recibe el porcentaje y devuelve una lambda que aplica dicho descuento a una cantidad. Esa lambda captura el valor del porcentaje, lo que constituye un cierre (closure), ya que la función resultante “recuerda” el valor con el que fue creada:

import java.util.function.Function;

public class Ejemplo {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return (cantidad) -> cantidad - (cantidad * porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento20 = crearDescuento(20);

        double precio = 100.0;

        System.out.println("Precio con 10%: " + descuento10.apply(precio));
        System.out.println("Precio con 20%: " + descuento20.apply(precio));
    }
}

En este ejemplo, crearDescuento genera funciones distintas en función del porcentaje proporcionado. Cada una de esas funciones mantiene internamente su propio valor de porcentaje, aunque la función original ya haya terminado su ejecución. Esto es posible gracias al cierre: la lambda captura la variable porcentaje del contexto donde fue definida, sin necesidad de recibirla como parámetro en cada llamada.

La closure permite así construir funciones parametrizadas de forma elegante y reutilizable. Cada función devuelta actúa como una “instancia” independiente con su propio estado (el porcentaje), lo que facilita crear comportamientos personalizados sin recurrir a clases adicionales ni estructuras más complejas.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional en Java es una interfaz que contiene exactamente un único método abstracto. Ese método representa la “firma funcional” que debe cumplir cualquier lambda que se asigne a ese tipo. En otras palabras, una interfaz funcional actúa como el tipo de una función, permitiendo que las lambdas sean compatibles con el sistema de tipos estático de Java. Ejemplos típicos en la biblioteca estándar son Function, Predicate o Consumer.

El requisito fundamental de una interfaz funcional es que solo puede tener un método abstracto. Puede, sin embargo, incluir otros métodos siempre que sean default o static, ya que estos no cuentan como abstractos. Además, es habitual (aunque no obligatorio) usar la anotación @FunctionalInterface, que indica al compilador que la interfaz está diseñada para ser funcional y ayuda a detectar errores si se añaden más métodos abstractos accidentalmente.

Este diseño permite que el compilador de Java pueda inferir el tipo de una lambda a partir del contexto. Es decir, una lambda no tiene un tipo propio independiente, sino que adopta el tipo de la interfaz funcional a la que se asigna. Por ejemplo, una expresión como (x) -> x + 1 puede ser válida como Function<Integer, Integer> o como otra interfaz funcional compatible, dependiendo del contexto.

En conjunto, las interfaces funcionales son el mecanismo que permite integrar programación funcional dentro de un lenguaje orientado a objetos con tipado estático como Java. Gracias a ellas, las lambdas no son elementos “aislados”, sino que se integran formalmente en el sistema de tipos, manteniendo la seguridad y coherencia del lenguaje.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Una interfaz funcional puede definirse manualmente en Java siempre que cumpla la condición de tener un único método abstracto. En este caso, se crea una interfaz llamada Transformador cuyo propósito es representar cualquier función que reciba una cadena de texto y devuelva otra cadena transformada. Este enfoque permite definir tipos funcionales propios, más allá de los ya incluidos en la biblioteca estándar como Function.

La interfaz se utiliza como tipo objetivo para lambdas, de forma que cualquier expresión lambda compatible con su firma pueda asignarse a una variable de tipo Transformador. Además, al tratarse de una interfaz funcional, puede utilizarse la anotación @FunctionalInterface, que refuerza la intención de diseño y permite al compilador validar que no se añaden más métodos abstractos.

A continuación se muestra la definición de la interfaz y un ejemplo de uso con una lambda que convierte una cadena a mayúsculas:

@FunctionalInterface
interface Transformador {
    String transformar(String texto);
}

public class Ejemplo {
    public static void main(String[] args) {

        Transformador aMayusculas = (texto) -> texto.toUpperCase();

        String resultado = aMayusculas.transformar("hola mundo");

        System.out.println(resultado);
    }
}

En este ejemplo, la interfaz Transformador actúa como el “tipo” de la función lambda. La lambda implementa implícitamente el método transformar, sin necesidad de crear una clase concreta. Este mecanismo muestra cómo Java integra el paradigma funcional dentro de su sistema de tipos estático, permitiendo definir abstracciones propias de forma sencilla y coherente.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Una interfaz funcional genérica permite abstraer aún más el comportamiento, ya que no se limita a un único tipo de entrada y salida. En lugar de fijar String -> String, se parametrizan los tipos mediante generics, de forma que el mismo concepto de transformador pueda reutilizarse para distintos tipos de datos. Esto encaja bien con el sistema de tipos de Java y con la idea de reutilización propia del paradigma funcional.

Al introducir genéricos, la interfaz pasa a definir dos tipos: uno de entrada y otro de salida. De este modo, se puede expresar cualquier transformación entre tipos sin necesidad de redefinir nuevas interfaces para cada caso. Este enfoque es equivalente a una función de orden superior tipada, donde el tipo de la función también es parametrizable.

La interfaz genérica Transformador puede definirse así:

@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}

En este caso, T representa el tipo de entrada y R el tipo de salida. A partir de esta definición, se puede crear un transformador que convierta un Double en un Integer, por ejemplo redondeando el valor:

public class Ejemplo {
    public static void main(String[] args) {

        Transformador<Double, Integer> redondear = (valor) -> (int) Math.round(valor);

        Double numero = 12.7;
        Integer resultado = redondear.transformar(numero);

        System.out.println(resultado);
    }
}

En este ejemplo, la lambda implementa la conversión de un tipo Double a Integer, adaptándose a la firma definida por la interfaz genérica. Esto permite reutilizar la misma interfaz para múltiples transformaciones distintas, manteniendo el código más flexible y expresivo.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java ya existe una colección amplia de interfaces funcionales predefinidas dentro del paquete java.util.function, diseñadas precisamente para evitar tener que crear interfaces genéricas como Transformador<T, R> en la mayoría de los casos. Estas interfaces cubren los usos más comunes del paradigma funcional (transformar valores, consumirlos, evaluarlos, combinarlos, etc.), y están totalmente integradas con las lambdas desde Java 8.

La más general es Function<T, R>, que representa una función que recibe un valor de tipo T y devuelve un resultado de tipo R, siendo equivalente al Transformador<T, R> del ejemplo anterior. A partir de esta base, existen otras interfaces especializadas que simplifican casos frecuentes y evitan sobrecarga de tipos genéricos cuando no es necesario.

Las principales interfaces funcionales predefinidas en Java son:

Function<T, R>: transforma un valor de tipo T en un valor de tipo R.
Predicate<T>: recibe un valor T y devuelve un boolean (evaluación de condición).
Consumer<T>: recibe un valor T y no devuelve nada (efecto lateral, consumo del dato).
Supplier<T>: no recibe parámetros y devuelve un valor de tipo T (proveedor de datos).
UnaryOperator<T>: caso particular de Function<T, T> (entrada y salida del mismo tipo).
BinaryOperator<T>: caso particular de BiFunction<T, T, T> (combina dos valores del mismo tipo).

Estas interfaces permiten expresar de forma muy clara distintos patrones funcionales sin necesidad de definir estructuras propias. Por ejemplo, Predicate<String> se usa para condiciones, Consumer<String> para acciones como imprimir, y Supplier<Double> para generar valores bajo demanda.

En conjunto, estas interfaces forman la base del soporte funcional en Java y permiten que las lambdas se integren directamente en el lenguaje de forma tipada, reutilizable y coherente con el resto del sistema de tipos.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método forEach en Java es un claro ejemplo de cómo el paradigma funcional sustituye a estructuras imperativas como el bucle for. En lugar de controlar explícitamente la iteración (índices, condiciones de parada, etc.), se pasa una función que se aplica a cada elemento de la colección. Esto hace que el código sea más declarativo, ya que se describe qué hacer con cada elemento en lugar de cómo recorrer la lista.

En este caso, se utiliza una lista de Integer y se aplica una lambda dentro de forEach para evaluar cada número. Si el valor es positivo, se muestra un mensaje. Este enfoque elimina la necesidad de escribir bucles explícitos y mejora la legibilidad en operaciones simples sobre colecciones.

A continuación se muestra un ejemplo en Java:

El método forEach en Java es un claro ejemplo de cómo el paradigma funcional sustituye a estructuras imperativas como el bucle for. En lugar de controlar explícitamente la iteración (índices, condiciones de parada, etc.), se pasa una función que se aplica a cada elemento de la colección. Esto hace que el código sea más declarativo, ya que se describe qué hacer con cada elemento en lugar de cómo recorrer la lista.

En este caso, se utiliza una lista de Integer y se aplica una lambda dentro de forEach para evaluar cada número. Si el valor es positivo, se muestra un mensaje. Este enfoque elimina la necesidad de escribir bucles explícitos y mejora la legibilidad en operaciones simples sobre colecciones.

A continuación se muestra un ejemplo en Java:

En este ejemplo, forEach recibe una lambda de tipo Consumer<Integer>, ya que consume cada elemento sin devolver nada. La lógica de filtrado (números positivos) se incluye directamente dentro de la función. Aunque en este caso aún se utiliza una condición if, el control del recorrido deja de ser responsabilidad del programador, lo que es característico del estilo funcional aplicado a colecciones en Java.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

En la firma de forEach se utiliza Consumer<? super T> en lugar de Consumer<T> debido a una regla fundamental del uso de genéricos en Java relacionada con la flexibilidad de tipos en tiempo de compilación. Un Consumer<? super T> significa que se acepta un consumidor de T o de cualquier supertipo de T. Esto permite mayor compatibilidad: por ejemplo, una List<Integer> puede aceptar un Consumer<Number> o Consumer<Object> porque ambos pueden consumir un Integer sin problema.

En cambio, si se utilizara Consumer<T> estrictamente, solo se permitiría un consumidor exactamente del mismo tipo, lo que reduciría la flexibilidad del API. Esta decisión está directamente relacionada con el principio PECS, que significa Producer Extends, Consumer Super. Este principio indica cómo deben usarse los comodines en genéricos: cuando un tipo produce valores, se usa extends; cuando consume valores, se usa super.

En el caso de forEach, la colección “produce” elementos de tipo T (los entrega uno a uno), mientras que el Consumer los “consume”. Por eso se usa ? super T, ya que lo importante es que el consumidor pueda aceptar ese tipo o uno más general.

Aplicando esto al método transformar, se puede mejorar la flexibilidad del tipo de la función transformadora. Originalmente se podría tener algo como:

Function<T, R>

Pero siguiendo PECS, se puede razonar así: la función consume un valor de tipo T, así que es correcto permitir tipos más generales como entrada. Por tanto, una versión más flexible sería:

Function<? super T, ? extends R>

Esto significa que:

La función puede aceptar T o cualquier supertipo de T como entrada (? super T).
Y puede devolver R o cualquier subtipo de R (? extends R).

Este diseño aumenta la reutilización de funciones, permitiendo pasar transformadores más generales o más específicos sin perder compatibilidad de tipos. Es la misma idea que en forEach: flexibilizar el consumo y la producción de tipos sin romper la seguridad del sistema de tipos de Java.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos permiten reutilizar directamente un método existente como si fuese una función, sin necesidad de envolverlo en una lambda explícita. En el fondo, son una forma abreviada de trabajar con funciones ya definidas, muy alineada con el paradigma funcional. En Java se introducen junto con las lambdas, y en JavaScript este comportamiento se logra normalmente mediante referencias directas a funciones de objetos.

En JavaScript, los métodos de un objeto pueden extraerse y almacenarse en variables, aunque es importante tener en cuenta el contexto de this, que puede perderse si no se enlaza correctamente. Un ejemplo sencillo con una clase Persona sería el siguiente:

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Ana");

// Referencia al método
const referenciaSaludar = persona.saludar.bind(persona);

// Invocación mediante la referencia
referenciaSaludar();

En este caso se utiliza bind para asegurar que el contexto this siga apuntando al objeto persona, ya que en JavaScript los métodos extraídos pierden su enlace original si no se fijan explícitamente.

En Java, las referencias a métodos son más estructuradas y están integradas con el sistema de tipos y las interfaces funcionales. Se utiliza el operador :: para obtener una referencia a un método. El ejemplo equivalente sería:

public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

import java.util.function.Consumer;

public class Ejemplo {
    public static void main(String[] args) {

        Persona persona = new Persona("Ana");

        // Referencia al método de instancia
        Consumer<Void> referenciaSaludar = (v) -> persona.saludar();

        // Invocación
        referenciaSaludar.accept(null);
    }
}

Una forma más idiomática en Java sería usar directamente una referencia a método compatible con Consumer<Persona>:

import java.util.function.Consumer;

public class Ejemplo {
    public static void main(String[] args) {

        Persona persona = new Persona("Ana");

        Consumer<Persona> referenciaSaludar = Persona::saludar;

        referenciaSaludar.accept(persona);
    }
}

En este caso, Persona::saludar representa el método como valor, sin necesidad de invocarlo explícitamente. Esto refuerza la idea de que los métodos pueden tratarse como funciones de primera clase dentro del ecosistema funcional de Java.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen cuatro tipos principales de referencias a métodos, todas ellas diseñadas para simplificar el uso de lambdas cuando ya existe un método compatible. Estas referencias permiten reutilizar código existente de forma más declarativa, manteniendo la integración con interfaces funcionales. Todas ellas se expresan mediante el operador ::, pero difieren en el contexto del método que se referencia.

El primer tipo es la referencia a método estático, que apunta directamente a un método de clase sin necesidad de instancia. El segundo es la referencia a constructor, que permite invocar un constructor como si fuera una función. El tercero es la referencia a método de instancia de un objeto concreto, donde se fija una instancia específica. Finalmente, el cuarto es la referencia a método de instancia de un tipo arbitrario, donde el objeto sobre el que se ejecuta el método se pasa como parámetro.

A continuación se muestran ejemplos de cada tipo:

1. Método estático

import java.util.function.Function;

public class Util {
    public static String duplicar(String texto) {
        return texto + texto;
    }

    public static void main(String[] args) {
        Function<String, String> ref = Util::duplicar;

        System.out.println(ref.apply("hola"));
    }
}

2. Referencia a constructor

import java.util.function.Supplier;

class Persona {
    String nombre;

    Persona() {
        this.nombre = "Sin nombre";
    }
}

public class Ejemplo {
    public static void main(String[] args) {
        Supplier<Persona> ref = Persona::new;

        Persona p = ref.get();
        System.out.println(p.nombre);
    }
}

3. Método de instancia de un objeto concreto

import java.util.function.Supplier;

class Persona {
    String nombre = "Ana";

    public void saludar() {
        System.out.println("Hola soy " + nombre);
    }
}

public class Ejemplo {
    public static void main(String[] args) {
        Persona persona = new Persona();

        Runnable ref = persona::saludar;

        ref.run();
    }
}

4. Método de instancia de cualquier objeto del tipo

import java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {

        Function<String, Integer> ref = String::length;

        System.out.println(ref.apply("hola mundo"));
    }
}

En conjunto, estos cuatro tipos de referencias permiten expresar de forma muy compacta distintas formas de reutilizar métodos existentes. Conceptualmente, todas se reducen a lambdas implícitas, pero con una sintaxis más clara cuando ya existe un método compatible, reforzando el estilo funcional dentro de Java.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

En este caso se trabaja con ordenación funcional, donde el criterio de comparación se define mediante una función en lugar de una clase tradicional. Esto permite que el comportamiento de Collections.sort sea flexible, ya que no depende de una única forma de ordenar, sino de la función que se le proporcione en cada caso. La idea es comparar primero la edad y, si coincide, comparar el nombre alfabéticamente.

Primero se muestra una versión “manual”, donde la lógica de comparación se implementa directamente dentro de una lambda pasada a Collections.sort. Esta lambda devuelve un valor negativo, cero o positivo según el resultado de la comparación, siguiendo el contrato del comparador:

import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}

public class Ejemplo {
    public static void main(String[] args) {

        List<Persona> lista = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Pedro", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(lista, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return p1.edad - p2.edad;
            } else {
                return p1.nombre.compareTo(p2.nombre);
            }
        });
    }
}

En esta versión, la lógica de comparación está escrita directamente en la lambda, lo que es expresivo pero puede volverse menos reutilizable si el criterio de ordenación se repite en varios lugares del programa.

La segunda versión utiliza la interfaz Comparator, que permite encapsular y reutilizar la lógica de comparación de forma más estructurada. Además, proporciona métodos auxiliares como comparing y thenComparing, que hacen el código más declarativo:

import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}

public class Ejemplo {
    public static void main(String[] args) {

        List<Persona> lista = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Pedro", 30),
            new Persona("Bea", 25)
        );

        Comparator<Persona> comparador =
            Comparator.comparingInt((Persona p) -> p.edad)
                      .thenComparing(p -> p.nombre);

        Collections.sort(lista, comparador);
    }
}

En esta segunda versión, Comparator permite expresar la lógica de ordenación de forma más legible y modular. Primero se define el criterio principal (edad) y después el secundario (nombre), lo que mejora la claridad del código. Este enfoque es más idiomático en Java moderno, ya que aprovecha las utilidades funcionales del propio lenguaje para evitar comparaciones manuales explícitas.