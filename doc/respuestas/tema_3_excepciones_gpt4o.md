# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al no existir excepciones como en Java, el control de errores debe diseñarse manualmente. Cuando una función como raiz recibe un número negativo, no puede “lanzar” un error automáticamente, por lo que debe indicarlo mediante algún mecanismo acordado entre la función y quien la llama. Es importante que el mensaje al usuario se gestione fuera de la función, de modo que esta solo calcule o informe del error, pero no decida cómo comunicarlo.

Una primera opción consiste en devolver un valor especial que indique error. En el caso de una raíz cuadrada, podría devolverse un valor imposible en condiciones normales (por ejemplo, -1 si se acuerda que solo se trabajará con resultados no negativos). El código que llama a la función comprobaría el valor devuelto antes de usarlo.

#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial que indica error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-9);

    if (resultado < 0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}

Una segunda opción más robusta consiste en devolver el resultado a través de un puntero y usar el valor de retorno de la función como código de estado. De este modo, la función puede devolver 0 si todo ha ido bien y 1 (u otro valor) si ha ocurrido un error. Esta técnica separa claramente el resultado del estado de la operación.

#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 1;  // código de error
    }
    *resultado = sqrt(x);
    return 0;  // sin error
}

int main() {
    float valor;

    if (raiz(-9, &valor) != 0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", valor);
    }

    return 0;
}

Ambas soluciones permiten que el control del error se realice fuera de la función raiz. La primera es más simple pero menos segura, ya que depende de un valor especial que podría confundirse con un resultado válido en otros contextos. La segunda es más clara y escalable, ya que separa el resultado del estado de ejecución, aproximándose más a los mecanismos de control de errores que posteriormente ofrecerán las excepciones en Java.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo de control de errores que permite indicar que durante la ejecución de un programa ha ocurrido una situación anómala que impide continuar con el flujo normal. A diferencia de C, donde el error debe indicarse mediante valores de retorno o códigos de estado, en Java una excepción es un objeto que se “lanza” cuando ocurre un problema y que puede ser capturado y gestionado en otro punto del programa.

El objetivo principal de las excepciones es separar claramente el código que realiza la tarea principal del código que gestiona los errores. De este modo, cuando se implementa una función, no es necesario devolver valores especiales ni mezclar constantemente comprobaciones de error con la lógica principal. En su lugar, si se detecta una situación incorrecta (por ejemplo, un parámetro inválido), se lanza una excepción y se delega la gestión del problema a quien haya llamado al método.

Cuando se llama a una función que puede producir errores, el programador utiliza excepciones para controlar esos posibles fallos de forma estructurada. Esto permite reaccionar ante distintos tipos de problemas (por ejemplo, error de entrada, acceso inválido, etc.) de manera específica y organizada. En resumen, las excepciones mejoran la claridad, la seguridad y la mantenibilidad del código al ofrecer un mecanismo formal para tratar errores.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el control de errores se realiza mediante excepciones en lugar de valores especiales o códigos de retorno como en C. En este caso, si el método raiz recibe un número negativo, puede lanzarse una excepción para indicar que el parámetro es inválido. De este modo, el método no se encarga de mostrar mensajes al usuario, sino únicamente de detectar la situación incorrecta y comunicarla mediante una excepción.

Se puede utilizar, por ejemplo, la excepción estándar IllegalArgumentException, que se emplea cuando un método recibe un argumento no válido. El método raiz se incluirá dentro de una clase Calculadora, y el control del error se realizará desde el método main utilizando un bloque try-catch, que permite capturar la excepción y actuar en consecuencia.

public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

En este diseño, el método raiz se limita a detectar el error y lanzar la excepción, mientras que el método main decide cómo gestionarlo. Esto permite separar la lógica del cálculo de la gestión del error, lo que hace que el código sea más claro y estructurado. Además, se evita el uso de valores especiales que podrían confundirse con resultados válidos, mejorando así la robustez del programa.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa generar un objeto de tipo excepción y enviarlo al sistema de ejecución cuando se detecta una situación anómala. En el ejemplo anterior, cuando el método raiz detecta que el número es negativo, ejecuta throw new IllegalArgumentException(...). En ese momento se interrumpe la ejecución normal del método y no se continúa con las instrucciones posteriores; en lugar de devolver un valor, se produce un salto automático fuera del método.

Controlar o capturar una excepción significa interceptarla mediante un bloque try-catch. En el ejemplo, el método main llama a Calculadora.raiz(-9) dentro de un bloque try, y el bloque catch captura la excepción IllegalArgumentException. Al capturarla, se evita que el programa termine abruptamente y se puede mostrar un mensaje adecuado o tomar una decisión alternativa. Es, por tanto, el punto donde se gestiona el problema detectado previamente.

Una excepción se propaga cuando el método que la recibe no la captura y la deja pasar al método que lo llamó. Si main no tuviera el bloque try-catch, la excepción lanzada en raiz subiría por la pila de llamadas hasta encontrar un método que la capture. Si ningún método la controla, el programa finaliza mostrando un mensaje de error. Durante esa propagación, cada método intermedio abandona su ejecución inmediatamente: se eliminan de la pila de llamadas y no continúan ejecutando las instrucciones que quedaban pendientes.

Las funciones que no controlan la excepción no se reanudan después. Una vez que la excepción abandona un método, su ejecución termina definitivamente. En el ejemplo, si raiz lanza la excepción, cualquier instrucción posterior dentro de ese método no se ejecuta. Del mismo modo, si hubiera otros métodos intermedios entre main y raiz, estos finalizarían automáticamente al propagarse la excepción. Solo el método que finalmente la capture podrá continuar la ejecución, pero desde el bloque catch, no desde el punto exacto donde se produjo el error.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

Una de las principales ventajas frente a C es que no es necesario comprobar manualmente el valor de retorno después de cada llamada a función. En C, cada función que llama a otra debe verificar si ha ocurrido un error y, en caso afirmativo, decidir si lo gestiona o lo devuelve al nivel superior. Esto genera código repetitivo y aumenta la probabilidad de olvidar una comprobación. En Java, si no se captura la excepción, esta se propaga automáticamente por la pila de llamadas hasta que algún método la controle.

Otra ventaja importante es que se separa claramente la lógica principal del programa del tratamiento de errores. En C, el código de control de errores suele mezclarse con la lógica funcional mediante múltiples condicionales (if). En cambio, con excepciones, el flujo normal del programa se escribe de forma más limpia, y la gestión de errores se agrupa en bloques catch, lo que mejora la legibilidad y el mantenimiento del código.

Además, la propagación natural garantiza que ningún error pase desapercibido. Si una excepción no es capturada en ningún punto, el programa finaliza mostrando información sobre el problema, lo que facilita la detección de fallos. En C, si no se comprueba correctamente un valor de retorno, el programa puede continuar en un estado inconsistente sin que el error sea evidente.

Por último, la propagación automática simplifica el diseño en programas con varias capas de llamadas. Un método intermedio que no sabe cómo resolver un problema no necesita inventar códigos de error ni transformarlos; simplemente deja que la excepción continúe su recorrido. Esto permite que el control del error se realice en el nivel más adecuado, normalmente el más externo, mejorando la organización y robustez del sistema.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En orientación a objetos, las excepciones son efectivamente objetos. En Java, todas las excepciones derivan de la clase Exception (y en general de la clase base Throwable). Esto significa que una excepción no es simplemente un código numérico o un mensaje, sino una instancia de una clase que puede contener atributos y métodos propios, igual que cualquier otro objeto que ya se ha trabajado al estudiar clases y encapsulación.

El hecho de que sean objetos aporta ventajas claras en términos de encapsulación. Una excepción puede almacenar información relevante sobre el error (por ejemplo, el valor incorrecto recibido, un mensaje descriptivo o cualquier otro dato útil) y ofrecer métodos para acceder a esa información de forma controlada. De este modo, el detalle interno del error queda protegido dentro del objeto excepción, y solo se expone lo que se considere necesario a través de métodos públicos, respetando el principio de ocultación de información.

Además, al ser clases, es posible crear excepciones personalizadas. Se puede definir una nueva clase que herede de Exception (o de alguna subclase existente) para representar errores específicos de una aplicación. Por ejemplo, podría crearse una excepción NumeroNegativoException para el caso de la raíz cuadrada. Esto permite modelar los errores de forma más precisa y coherente con el diseño orientado a objetos.

En consecuencia, las excepciones no solo sirven para interrumpir la ejecución, sino que forman parte del modelo del programa. Al poder definir tipos propios de excepción, se consigue un código más expresivo, más organizado y más alineado con los principios de la programación orientada a objetos.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En comparación con C, donde normalmente solo se dispone de un código de error o un valor especial, en Java cualquier objeto excepción contiene información estructurada sobre el error ocurrido. La primera información esencial es el mensaje descriptivo, que explica qué ha sucedido. Este mensaje se define al crear la excepción (por ejemplo, al hacer throw new IllegalArgumentException("...")) y puede recuperarse en el manejador mediante el método getMessage(). Esto permite que el punto donde se captura la excepción disponga de una explicación clara del problema.

Otra información fundamental que contiene toda excepción es el tipo concreto de la excepción. No es lo mismo capturar una IllegalArgumentException que una NullPointerException. El propio tipo de objeto ya clasifica el error, lo que permite manejar situaciones distintas de forma diferente mediante varios bloques catch. En C, en cambio, esa diferenciación suele hacerse manualmente mediante códigos numéricos, lo que es menos expresivo y más propenso a errores.

Además, toda excepción incorpora automáticamente la traza de la pila de llamadas (stack trace). Esta traza indica qué métodos estaban activos en el momento en que se produjo el error y en qué línea ocurrió. Cuando la excepción llega al manejador, esta información resulta muy útil para depurar el programa, ya que permite reconstruir el camino que siguió la ejecución hasta el fallo.

Por tanto, frente a C, donde el error suele reducirse a un valor devuelto, en Java el objeto excepción encapsula mensaje, tipo y contexto de ejecución. Esta combinación proporciona al manejador información mucho más rica y estructurada, lo que facilita tanto la gestión adecuada del error como la localización y corrección de fallos.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java, sí se pueden tener varios bloques catch asociados a un mismo bloque try. Cada bloque catch está diseñado para capturar un tipo específico de excepción o cualquiera de sus subclases. Esto permite manejar de manera diferenciada distintos tipos de errores que puedan producirse dentro del mismo bloque try, sin necesidad de mezclar la lógica de tratamiento de cada error en un solo bloque.

Sin embargo, solo se ejecuta un bloque catch por cada excepción que se produzca. Cuando se lanza una excepción, el sistema busca el primer bloque catch cuyo tipo coincida con el de la excepción (o sea compatible mediante herencia). Una vez que se encuentra ese bloque, se ejecuta y el resto de los bloques catch asociados al mismo try se omiten. Por eso, el orden de los bloques catch es importante: los tipos más específicos deben colocarse antes que los más generales, como Exception, para que las excepciones no queden atrapadas por un bloque genérico antes de llegar a uno específico.

Por ejemplo, si dentro de un try se lanza una IllegalArgumentException, y hay un bloque catch para IllegalArgumentException seguido de otro para Exception, solo se ejecutará el primero, y el segundo se ignorará. Esto garantiza un control preciso y evita que una excepción específica se maneje incorrectamente como un error general.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

En Java, para garantizar que siempre se ejecute un bloque de código necesario, como el cierre de ficheros o la liberación de recursos, se utiliza el bloque finally. Este bloque se ejecuta siempre, independientemente de que se produzca una excepción o no, y también aunque la excepción se propague sin ser capturada. Esto permite separar la gestión de recursos del manejo del error y asegura que el programa no deje recursos abiertos o en estado inconsistente.

Cuando se combina con un bloque catch, el flujo de ejecución es: primero se intenta el código dentro del try; si ocurre una excepción, se ejecuta el bloque catch correspondiente; y, finalmente, siempre se ejecuta el bloque finally. Incluso si el bloque catch no captura la excepción, el finally se ejecuta antes de que la excepción siga propagándose.

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class EjemploFinally {
    public static void main(String[] args) {
        Scanner sc = null;
        try {
            sc = new Scanner(new File("archivo.txt"));
            System.out.println("Leyendo archivo...");
            // aquí podría lanzarse una excepción
        } catch (FileNotFoundException e) {
            System.out.println("Archivo no encontrado: " + e.getMessage());
        } finally {
            if (sc != null) {
                sc.close();  // se cierra siempre
                System.out.println("Recurso liberado");
            }
        }
    }
}

También es posible usar finally sin un bloque catch, dejando que la excepción se propague al llamador. Esto es útil cuando el método no puede manejar el error pero necesita asegurar la liberación de recursos:

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class EjemploFinallySinCatch {
    public static void main(String[] args) throws FileNotFoundException {
        Scanner sc = null;
        try {
            sc = new Scanner(new File("archivo.txt"));
            System.out.println("Leyendo archivo...");
            // si ocurre FileNotFoundException, se propaga
        } finally {
            if (sc != null) {
                sc.close();  // se cierra siempre
                System.out.println("Recurso liberado");
            }
        }
    }
}

En ambos casos, el bloque finally garantiza que los recursos se liberen correctamente antes de continuar, ya sea capturando la excepción o permitiendo que ésta suba por la pila de llamadas. Esto evita fugas de recursos y asegura la consistencia del programa.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, en Java un bloque finally puede ir acompañado únicamente de un try sin necesidad de tener un catch. Esto se utiliza cuando un método no puede o no quiere capturar la excepción, pero necesita asegurar que ciertos recursos se liberen o que ciertas operaciones se realicen siempre, independientemente de que se produzca un error.

El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no ocurre ninguna. Incluso si la excepción se propaga sin ser capturada, el finally se ejecuta antes de que la excepción continúe su camino por la pila de llamadas. Esto garantiza la limpieza de recursos y la consistencia del programa en cualquier circunstancia.

Incluso si dentro del bloque try hay un return, el bloque finally se ejecuta antes de que el método devuelva el control al llamador. Esto significa que el finally tiene prioridad para realizar su código de limpieza antes de que el flujo normal del método se complete, asegurando que nada se quede sin liberar o ejecutar, aunque se haya decidido salir del método anticipadamente.

Ejemplo:
public class FinallyReturn {
    public static int ejemplo() {
        try {
            System.out.println("Dentro del try");
            return 10;
        } finally {
            System.out.println("Ejecutando finally antes del return");
        }
    }

    public static void main(String[] args) {
        int valor = ejemplo();
        System.out.println("Valor devuelto: " + valor);
    }
}

Salida esperada:
Dentro del try
Ejecutando finally antes del return
Valor devuelto: 10

Este comportamiento asegura que el finally es confiable para liberar recursos o ejecutar instrucciones críticas, incluso frente a retornos anticipados o excepciones que se propaguen.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las excepciones se dividen en controladas (checked) y no controladas (unchecked), según la obligación que tiene el programador de declararlas o capturarlas. Las excepciones controladas son aquellas que el compilador obliga a manejar: si un método puede lanzarlas, debe declararse con throws, y quien llame a ese método debe capturarlas o propagarlas. Estas suelen ocurrir por factores externos al programa, como errores de entrada/salida o problemas de red, y permiten que el código cliente reaccione de forma previsiblemente segura.

Por otro lado, las excepciones no controladas no necesitan ser declaradas ni obligatoriamente capturadas. Estas heredan de RuntimeException y suelen reflejar errores de programación que no se esperan en condiciones normales, como referencias nulas, divisiones por cero o accesos a índices fuera de rango. RuntimeException funciona como base de este tipo de excepciones, permitiendo que ocurran de forma espontánea durante la ejecución y que el programador decida si desea capturarlas.

Ejemplos típicos de cada tipo pueden ser:

Excepciones controladas:

    --> IOException → errores al leer o escribir archivos.

    --> FileNotFoundException → archivo que no existe al intentar abrirlo.

    --> SQLException → errores en consultas a bases de datos.

    --> ClassNotFoundException → al cargar clases dinámicamente.

Excepciones no controladas:

    --> NullPointerException → acceso a un objeto nulo.

    --> ArithmeticException → división por cero.

    --> ArrayIndexOutOfBoundsException → acceso a posiciones inválidas en un arreglo.

    --> IllegalArgumentException → argumento inválido pasado a un método.

Se suele preferir una excepción controlada en situaciones donde el fallo depende de factores externos y es posible recuperarse, por ejemplo:

    1. Abrir un archivo que puede no existir.

    2. Conectarse a una base de datos remota que puede fallar.

    3. Leer datos de un socket o flujo de red.

    4. Cargar un recurso externo que puede no estar disponible.

Se suele preferir una excepción no controlada cuando el error refleja un fallo de programación que no se espera en condiciones normales:

    1. Acceder a un objeto que podría ser nulo.

    2. Dividir un número por cero.

    3. Acceder a un índice fuera de los límites de un arreglo o lista.

    4. Pasar argumentos inválidos a un método (por ejemplo, negativos donde no se admiten).

Este enfoque permite que las excepciones controladas promuevan la gestión explícita de fallos previsibles, mientras que las no controladas reflejan errores de lógica que deben corregirse en el desarrollo.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

En Java, la palabra clave throws se utiliza en la declaración de un método para indicar que dicho método puede generar una o varias excepciones controladas durante su ejecución. Básicamente, informa al compilador y a quien llame al método que debe estar preparado para manejar esas excepciones, ya sea capturándolas con un bloque try-catch o propagándolas a su vez con otro throws. Esto permite que la función no tenga que encargarse directamente del manejo del error, dejando la decisión a quien la invoque.

El uso de throws es una alternativa a capturar la excepción dentro del propio método. En lugar de escribir un bloque try-catch dentro de la función, se puede delegar la responsabilidad al llamador, simplificando la lógica interna del método y permitiendo que la gestión de errores se haga en un nivel más adecuado o más externo. Esto es útil cuando el método no tiene suficiente contexto para decidir cómo reaccionar ante la excepción o cuando se quiere centralizar el control de errores en un único punto del programa.

Por ejemplo, al trabajar con archivos, un método que abre un archivo podría declararse así:

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class EjemploThrows {
    public static void leerArchivo(String nombreArchivo) throws FileNotFoundException {
        Scanner sc = new Scanner(new File(nombreArchivo));
        System.out.println("Archivo abierto correctamente");
        sc.close();
    }

    public static void main(String[] args) {
        try {
            leerArchivo("archivo.txt");
        } catch (FileNotFoundException e) {
            System.out.println("No se encontró el archivo: " + e.getMessage());
        }
    }
}

En este ejemplo, el método leerArchivo no captura la excepción FileNotFoundException internamente; en cambio, la declara con throws, indicando que quien lo llame debe gestionarla. Esto separa claramente la lógica de abrir el archivo de la lógica de manejar errores, haciendo el código más modular y fácil de mantener.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En Java, un método puede declararse con throws para indicar que no se encargará de capturar ciertas excepciones controladas, dejando que se propaguen hacia el llamador. En el caso de abrir un fichero que puede no existir, la excepción típica es FileNotFoundException. Aunque el método no la capture, se puede usar un bloque finally para garantizar que se liberen recursos o se cierre cualquier objeto abierto, incluso si ocurre la excepción.

Un ejemplo de este diseño sería:

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class LecturaArchivo {

    // Método que declara que puede lanzar FileNotFoundException
    public static void abrirArchivo(String nombreArchivo) throws FileNotFoundException {
        Scanner sc = null;
        try {
            sc = new Scanner(new File(nombreArchivo));
            System.out.println("Archivo abierto correctamente");
            // Aquí se podrían realizar lecturas
        } finally {
            if (sc != null) {
                sc.close(); // Siempre se cierra, incluso si ocurre la excepción
                System.out.println("Recurso liberado en finally");
            }
        }
    }

    public static void main(String[] args) {
        try {
            abrirArchivo("archivo.txt");  // La excepción se propaga al main
        } catch (FileNotFoundException e) {
            System.out.println("Error en main: archivo no encontrado - " + e.getMessage());
        }
    }
}

En este ejemplo:

    1. El método abrirArchivo no captura la excepción FileNotFoundException; la declara con throws para que se propague hacia arriba.

    2. El bloque finally asegura que el recurso (Scanner) se cierre correctamente, evitando fugas incluso si ocurre el error.

    3. En el main, se captura la excepción para mostrar un mensaje al usuario, mostrando cómo la propagación permite separar la lógica de apertura del archivo de la lógica de manejo de errores.

Este patrón es útil cuando un método no tiene suficiente contexto para manejar un error, pero aún así necesita garantizar la liberación de recursos.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, en Java se puede poner en throws excepciones no controladas, como RuntimeException o cualquiera de sus subclases, pero no es obligatorio. El compilador no exige que las excepciones no controladas se capturen ni se declaren, porque se consideran errores de programación que deberían evitarse en lugar de manejarse rutinariamente.

Si un método declara que puede lanzar una RuntimeException, el llamador no está obligado a envolver la llamada en un try-catch. Aun así, podría hacerlo si se desea reaccionar a esa situación de forma específica, por ejemplo, registrando el error, mostrando un mensaje al usuario o tomando medidas preventivas para mantener la aplicación en un estado seguro. Sin embargo, capturar sistemáticamente excepciones no controladas suele ser un signo de diseño deficiente, porque estas excepciones representan normalmente errores de lógica que deberían corregirse, no manejarse en tiempo de ejecución.

El sentido de declarar o capturar una RuntimeException es principalmente informativo o preventivo. Puede servir para:

    1. Documentar en la firma del método que hay situaciones potenciales de error graves (aunque el compilador no lo exija).

    2. Permitir a un llamador avanzado interceptar excepciones que, aunque no deberían ocurrir, podrían suceder en circunstancias especiales (por ejemplo, datos corruptos o condiciones inesperadas en la entrada).

Ejemplo ilustrativo:

public class EjemploRuntime {
    public static void division(int a, int b) throws ArithmeticException { // opcional
        if (b == 0) {
            throw new ArithmeticException("División por cero");
        }
        System.out.println("Resultado: " + (a / b));
    }

    public static void main(String[] args) {
        try {
            division(10, 0);  // capturar RuntimeException es opcional
        } catch (ArithmeticException e) {
            System.out.println("Error controlado en main: " + e.getMessage());
        }
    }
}

En este caso, declarar throws ArithmeticException no es necesario, porque es una excepción no controlada, pero se puede hacer para documentar intencionalmente que el método puede lanzar esa excepción. Capturarla en main es opcional y sirve solo si se desea gestionar el error sin que el programa termine abruptamente.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar excepciones controladas cuando el error depende de factores externos al programa y es posible que el llamador pueda recuperarse de él. Por ejemplo, fallos al leer o escribir archivos (IOException), problemas de red, errores al conectarse a bases de datos o recursos externos que pueden no estar disponibles. En estos casos, la excepción obliga al programador a considerar la posibilidad de error y a gestionarlo explícitamente mediante try-catch o throws, lo que aumenta la seguridad y robustez del programa.

En cambio, las excepciones no controladas, como IllegalArgumentException o NullPointerException, se utilizan para errores de lógica interna o de programación, donde no tiene sentido que el llamador intente recuperarse. Por ejemplo, pasar un argumento inválido a un método o intentar operar sobre un objeto nulo. Estas excepciones reflejan fallos que deberían corregirse en el código y no dependen de condiciones externas, por lo que no se obligan a capturar.

No todos los lenguajes proporcionan ambas opciones. Por ejemplo, C y C++ sin orientación a objetos no tienen excepciones controladas como Java; normalmente los errores se comunican mediante códigos de retorno o valores especiales. En estos lenguajes, lo más habitual es usar una única estrategia de manejo de errores: códigos de retorno, banderas o punteros nulos, que el llamador debe revisar explícitamente. Lenguajes como Java, en cambio, distinguen formalmente entre excepciones controladas y no controladas, ofreciendo mayor claridad y control sobre qué errores deben gestionarse obligatoriamente y cuáles representan fallos de programación.

En resumen, las controladas se usan para errores previsibles y externos, las no controladas para errores internos y lógicos, y en lenguajes sin este mecanismo se suele recurrir a códigos de estado revisables por el llamador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido lanzar excepciones dentro de un bloque catch. Esto se hace cuando el manejador detecta que no puede resolver completamente el error o quiere informar a un nivel superior de que ha ocurrido un problema. En ese caso, puede lanzarse una nueva excepción, diferente de la original, que aporte más contexto o represente un error más específico para el llamador.

Por ejemplo, si un método de lectura de archivo captura un FileNotFoundException, podría lanzar una excepción personalizada ArchivoNoDisponibleException para encapsular el error de manera más significativa para el sistema:

try {
    Scanner sc = new Scanner(new File("archivo.txt"));
} catch (FileNotFoundException e) {
    throw new RuntimeException("Error al acceder al recurso requerido", e);
}

En Java también es posible relanzar la misma excepción capturada usando simplemente throw e;. Esto es útil cuando se desea realizar alguna acción parcial en el catch (por ejemplo, registrar el error, liberar recursos o notificar al usuario) pero luego dejar que la excepción siga propagándose hacia arriba, sin que el método intermedio la absorba completamente.

Ejemplo de relanzar la misma excepción:

try {
    Scanner sc = new Scanner(new File("archivo.txt"));
} catch (FileNotFoundException e) {
    System.out.println("Se intentó abrir un archivo que no existe: " + e.getMessage());
    throw e; // se relanza para que el llamador lo gestione
}

En este caso, el bloque catch realiza una acción adicional (mensaje o registro) pero no suprime la excepción; permite que un nivel superior decida cómo manejarla. Esta práctica es útil en sistemas donde varios niveles de la pila de llamadas deben tener información sobre el error, o cuando se quiere añadir contexto sin bloquear la propagación natural de la excepción.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Que una excepción sea la “causa” de otra significa que una excepción de nivel superior envuelve o encapsula otra de nivel inferior, de manera que el segundo error explica por qué se produjo el primero. Esto es útil para ocultar detalles de implementación interna a un nivel más alto de la aplicación, mientras se mantiene el rastro completo del error original. En Java, esto se hace pasando la excepción original como argumento al constructor de la nueva excepción, usando por ejemplo new MiExcepcion("mensaje", causa).

Por ejemplo, se puede capturar un FileNotFoundException en un método de bajo nivel y encapsularlo en una excepción personalizada de alto nivel ArchivoNoDisponibleException, que represente un fallo general en la disponibilidad de un recurso dentro de la aplicación:

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

// Excepción personalizada de alto nivel
class ArchivoNoDisponibleException extends Exception {
    public ArchivoNoDisponibleException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {
    public static void abrirArchivo(String nombreArchivo) throws ArchivoNoDisponibleException {
        try {
            Scanner sc = new Scanner(new File(nombreArchivo));
        } catch (FileNotFoundException e) {
            // encapsulamos la excepción de bajo nivel en una de alto nivel
            throw new ArchivoNoDisponibleException("No se pudo abrir el archivo", e);
        }
    }

    public static void main(String[] args) {
        try {
            abrirArchivo("archivo.txt");
        } catch (ArchivoNoDisponibleException e) {
            e.printStackTrace();  // imprime la excepción y su causa
        }
    }
}

Cuando se imprime una excepción que tiene una causa, printStackTrace() muestra tanto el rastro de la excepción de alto nivel como la de bajo nivel que la originó. Aparecerá algo como:

ArchivoNoDisponibleException: No se pudo abrir el archivo
    at EjemploCausa.abrirArchivo(EjemploCausa.java:12)
Caused by: java.io.FileNotFoundException: archivo.txt (No such file or directory)
    at java.base/java.io.FileInputStream.open0(Native Method)
    at java.base/java.io.FileInputStream.open(FileInputStream.java:216)
    ...

Esto proporciona información completa del fallo, combinando la visión de alto nivel para la lógica de negocio con los detalles técnicos del error de bajo nivel, sin perder contexto.
