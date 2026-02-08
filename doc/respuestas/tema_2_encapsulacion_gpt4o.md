# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación en programación orientada a objetos persigue agrupar los datos y el comportamiento relacionado dentro de una misma entidad, el objeto. La ocultación de información complementa este concepto restringiendo el acceso directo a los detalles internos del objeto.

El objetivo principal es evitar que el estado interno de un objeto pueda ser modificado de forma arbitraria desde el exterior. De este modo, se garantiza que los cambios en los datos se realicen únicamente a través de operaciones controladas.

Entre las ventajas de la ocultación destacan una mayor robustez del programa, la reducción de errores, la posibilidad de cambiar la implementación interna sin afectar al código cliente y una mejora clara en el mantenimiento y la evolución del software.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de una clase es el conjunto de métodos y atributos accesibles desde el exterior. Representa el contrato que la clase ofrece a otras partes del programa para interactuar con sus objetos.

Esta interfaz define qué se puede hacer con un objeto, pero no cómo se hace. La implementación interna permanece oculta, lo que permite modificarla sin romper el funcionamiento del código que utiliza la clase.

La ocultación de información se apoya precisamente en esta separación entre interfaz pública e implementación privada, favoreciendo un diseño más limpio y desacoplado.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública debe diseñarse con cuidado porque es el punto de contacto entre la clase y el resto del sistema. Cualquier decisión tomada en ella condiciona el uso futuro de la clase.

Una vez que una clase es utilizada por otros módulos, cambiar su interfaz pública puede resultar costoso y provocar errores en cascada. Métodos eliminados o modificados obligan a revisar todo el código dependiente.

Por ello, se considera buena práctica exponer solo lo necesario y mantener la interfaz lo más estable y simple posible desde el inicio.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto se considere válido. Estas reglas definen estados correctos y coherentes del objeto.

Por ejemplo, una clase Punto podría imponer que sus coordenadas no sean valores no numéricos. Mantener estas condiciones es fundamental para el correcto funcionamiento del programa.

La ocultación de información ayuda a preservar las invariantes, ya que impide modificaciones directas de los atributos y obliga a utilizar métodos que pueden validar los cambios.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de esta clase está formada por el constructor y el método calcularDistanciaAOrigen. Los atributos x e y no forman parte de la interfaz.

El modificador public indica que el elemento es accesible desde cualquier clase. El modificador private restringe el acceso únicamente al interior de la propia clase.

Esta separación protege el estado interno del objeto y obliga a interactuar con él de forma controlada.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private pueden aplicarse a clases, atributos, métodos y constructores.

Una clase public puede utilizarse desde cualquier paquete, mientras que una clase private solo puede declararse como clase interna. Los atributos y métodos private solo son accesibles dentro de su propia clase.

Esta granularidad permite un control muy preciso sobre la visibilidad y el acceso a los distintos elementos de una clase.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de public y private, existen otros niveles de visibilidad. En Java se dispone de cuatro: public, private, protected y visibilidad por defecto (package-private).

protected permite el acceso desde subclases y desde clases del mismo paquete. La visibilidad por defecto restringe el acceso al paquete.

Otros lenguajes ofrecen modelos similares, aunque con nombres distintos. En C++, por ejemplo, existen public, private y protected.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Una instancia puede acceder a los atributos privados de otra instancia del mismo tipo

public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}


Esto es posible porque el acceso se realiza desde el código de la misma clase, no desde una clase externa.

La encapsulación se mantiene, ya que ninguna clase ajena puede acceder directamente a esos atributos.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter permiten obtener el valor de un atributo privado, mientras que los setter permiten modificarlo de forma controlada.

Se utilizan para mantener la ocultación de información, ofreciendo acceso indirecto a los datos internos. Esto permite validar valores o realizar acciones adicionales.

No todos los atributos necesitan getters y setters. Su uso debe justificarse por una necesidad real de acceso externo.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se afirma que la ocultación mejora la “seguridad”, no se hace referencia a la protección frente a ataques externos o hacking.

Se trata de seguridad lógica del programa, evitando estados inconsistentes, accesos indebidos y modificaciones accidentales de los datos.

Este tipo de seguridad mejora la fiabilidad y estabilidad del software, reduciendo errores difíciles de detectar.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia pertenece a cada objeto individualmente. Cada instancia tiene su propia copia del atributo.

Un miembro de clase pertenece a la clase en sí y es compartido por todas las instancias. En Java se indica mediante static.

Los miembros de clase también pueden ocultarse utilizando private, igual que los miembros de instancia.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Tiene sentido que un constructor sea privado cuando se quiere impedir la creación directa de objetos desde fuera de la clase.

Esto se utiliza, por ejemplo, en patrones de diseño como el Singleton o en clases factoría.

De este modo, se controla completamente cómo y cuántas instancias se crean.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

Sí, se ha utilizado static porque el método pertenece a la clase y no a una instancia concreta.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}


La interfaz pública se mantiene intacta. El cambio afecta únicamente a la implementación interna.

Esto demuestra el valor de la encapsulación: el código cliente no necesita modificarse.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Declarar un atributo público elimina cualquier control sobre su modificación. Un setter permite validar valores y preservar invariantes.

La convención más habitual es declarar los atributos como private y proporcionar solo los accesos necesarios.

Esto está directamente relacionado con las invariantes, ya que se evita que el objeto entre en estados inválidos.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando su estado no puede cambiar tras su creación. No existen métodos que modifiquen sus atributos.

Un método modificador es aquel que altera el estado del objeto. No todos los métodos modificadores son setters, aunque muchos lo sean.

Las clases inmutables ofrecen ventajas como simplicidad, seguridad frente a errores y facilidad para trabajar en entornos concurrentes.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir setters siempre por defecto. Cada setter expone la posibilidad de modificar el estado del objeto.

Solo deben incluirse cuando tenga sentido desde el punto de vista del dominio del problema.

Un diseño cuidadoso prioriza la coherencia del objeto frente a la comodidad de acceso.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Al concatenar dos cadenas, se crea un nuevo objeto String.

Esto puede resultar ineficiente si se realizan muchas concatenaciones sucesivas.

En esos casos, se recomienda utilizar clases como StringBuilder, que permiten modificaciones eficientes.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

Los objetos pueden compararse por identidad o por contenido. La identidad comprueba si son el mismo objeto en memoria.

El método equals en Java compara el contenido lógico. Por defecto, se comporta como ==, comparando identidad.

Las cadenas deben compararse siempre con equals, no con ==, para comparar su contenido correctamente.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper envuelven tipos primitivos en objetos. Ejemplos en Java son Integer, Double o Boolean.

Java realiza este proceso automáticamente mediante autoboxing y unboxing.

Permiten tratar valores primitivos como objetos, facilitando su uso en colecciones y APIs genéricas. No todos los lenguajes necesitan wrappers.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un enumerado es un tipo de dato que define un conjunto finito de valores posibles.

En Java, un enumerado es una clase especial que puede tener atributos, métodos y constructores.

Esto mejora la encapsulación, ya que se asocian comportamiento y datos a cada valor del enumerado.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        return esHemisferioNorte ? (ordinal >= 3 && ordinal <= 5) : (ordinal >= 9 && ordinal <= 11);
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        return esHemisferioNorte ? (ordinal >= 6 && ordinal <= 8) : (ordinal == 12 || ordinal <= 2);
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        return esHemisferioNorte ? (ordinal >= 9 && ordinal <= 11) : (ordinal >= 3 && ordinal <= 5);
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        return esHemisferioNorte ? (ordinal == 12 || ordinal <= 2) : (ordinal >= 6 && ordinal <= 8);
    }
}
