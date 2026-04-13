# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo en programación orientada a objetos se define como la capacidad de un mismo elemento (por ejemplo, una variable de tipo clase padre) de comportarse de diferentes formas en función del objeto real al que haga referencia en tiempo de ejecución. Esto permite trabajar con una interfaz común para distintos tipos concretos, sin necesidad de conocer explícitamente cuál es el tipo específico en cada caso.

Su utilidad principal es aumentar la flexibilidad y extensibilidad del código, ya que se pueden añadir nuevas clases que comparten una misma jerarquía sin modificar el código que las utiliza. De esta forma, se facilita el mantenimiento y se reduce el acoplamiento entre componentes del sistema.

La sobreescritura de métodos (method overriding) es un mecanismo mediante el cual una clase hija proporciona una implementación específica de un método que ya está definido en su clase padre. Cuando se llama a ese método a través de una referencia de la clase padre, se ejecuta la versión de la clase hija si el objeto real pertenece a ella, lo cual es una de las bases del polimorfismo en Java.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica (o enlace tardío) consiste en decidir en tiempo de ejecución qué implementación concreta de un método se va a ejecutar, en lugar de decidirlo en tiempo de compilación. Esto implica que la llamada a un método no se resuelve únicamente por el tipo de la variable, sino por el tipo real del objeto que está almacenado en esa referencia.

Su relación con el polimorfismo es directa, ya que es el mecanismo que permite que una misma llamada a un método produzca comportamientos distintos según el objeto concreto. Sin ligadura dinámica, el polimorfismo de subtipos no podría funcionar, porque siempre se ejecutaría la versión del método asociada al tipo estático de la referencia.

En Java, la ligadura dinámica es el comportamiento habitual para los métodos de instancia (excepto los métodos static, final o private), y no se necesita indicar explícitamente. En C++, en cambio, es necesario usar la palabra clave virtual en la clase base para activar el comportamiento polimórfico en tiempo de ejecución; si no se usa, se aplica ligadura estática.

En Python, la resolución es siempre dinámica por defecto debido a su naturaleza interpretada y de tipado dinámico. Esto significa que cualquier método se resuelve en tiempo de ejecución sin necesidad de palabras clave adicionales, lo que hace que el polimorfismo sea implícito y más flexible, aunque también puede reducir ciertas comprobaciones en tiempo de compilación.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Se solicita un ejemplo en Java que ilustre el polimorfismo mediante una clase base Soldado y dos subclases Zapador y Artillero. En este caso, se define en la clase base un método saludar, que es sobreescrito en una de las subclases para modificar completamente su comportamiento, lo que permite observar la resolución dinámica de métodos.

A continuación se muestra un ejemplo sencillo donde se crea un array de tipo Soldado, pero que contiene objetos de distintos tipos concretos. Al recorrer dicho array y llamar al método saludar, se ejecuta la versión correspondiente al tipo real del objeto, lo que demuestra el funcionamiento del polimorfismo.

class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un zapador especializado en ingeniería.");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un artillero especializado en cañones.");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[2];

        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

Este comportamiento se debe a la ligadura dinámica en Java, donde aunque la referencia sea de tipo Soldado, la ejecución del método saludar depende del objeto real almacenado en cada posición del array.

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, al sobreescribir un método es posible invocar la implementación de la clase base y construir un nuevo comportamiento a partir de ella. Esto se utiliza cuando no se desea reemplazar completamente el método, sino extenderlo o modificarlo parcialmente, reutilizando parte de su lógica original.

En Java, esta invocación al método de la clase padre se realiza mediante la palabra clave super. De este modo, dentro del método sobreescrito se puede llamar a super.metodo() para ejecutar primero (o en algún punto) el comportamiento definido en la superclase.

En el caso propuesto, Zapador puede llamar al método saludar de Soldado y después añadir su propio mensaje adicional. Esto permite mantener el saludo base y extenderlo sin duplicar código, lo cual es una práctica habitual en herencia cuando se quiere especializar un comportamiento.

class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

En la sobreescritura de métodos en Java, los parámetros deben ser exactamente los mismos que los del método de la clase base. Esto implica que no se pueden modificar ni el número, ni el tipo, ni el orden de los parámetros, ya que de lo contrario no se estaría sobrescribiendo el método, sino definiendo otro distinto. La firma del método debe coincidir completamente para que se considere una sobreescritura válida.

Respecto al tipo de retorno, se permite cierta flexibilidad gracias a la covarianza: el método sobrescrito puede devolver el mismo tipo o un subtipo del tipo de retorno definido en la clase padre. Sin embargo, no está permitido cambiarlo a un tipo no compatible, ya que rompería la coherencia del polimorfismo.

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es que la sobreescritura ocurre entre una clase padre y una clase hija, manteniendo la misma firma del método y redefiniendo su comportamiento. En cambio, la sobrecarga ocurre dentro de la misma clase (o jerarquía), definiendo varios métodos con el mismo nombre pero con distintos parámetros, y se resuelve en tiempo de compilación.

La anotación @Override sirve para indicar explícitamente que un método está siendo sobrescrito. Su uso es recomendable porque permite al compilador verificar que realmente existe un método equivalente en la clase padre. Esto ayuda a evitar errores comunes, como escribir mal el nombre del método o modificar accidentalmente su firma, lo que impediría la sobreescritura sin que el programador lo detecte fácilmente.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

En el estudio de Java, el polimorfismo no suele introducirse de forma explícita desde el primer momento como concepto teórico avanzado, pero sí se utiliza de manera progresiva desde etapas muy tempranas. Desde que se empieza a trabajar con clases, objetos y métodos heredados de Object, ya se está utilizando polimorfismo de forma implícita sin necesariamente nombrarlo como tal.

Cuando se sobrescriben métodos como toString() o equals(), efectivamente ya se está haciendo uso de polimorfismo. En estos casos, se está redefiniendo el comportamiento de métodos que originalmente están declarados en la clase base Object, de forma que el mismo método puede comportarse de manera distinta dependiendo de la clase concreta del objeto.

Esto significa que, aunque no se estén utilizando jerarquías complejas ni referencias de tipo padre apuntando a objetos hijos de forma explícita, la base del polimorfismo (la sobreescritura y la ligadura dinámica) ya está presente. Por ejemplo, cuando se imprime un objeto con System.out.println(obj), se invoca dinámicamente el toString() correspondiente al tipo real del objeto.

Por tanto, puede decirse que el polimorfismo no es algo que se “empiece a usar más adelante”, sino una característica fundamental de Java que está presente desde los primeros ejemplos prácticos, aunque su explicación formal y su uso avanzado (con herencia y colecciones de tipos base) se desarrollen posteriormente.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no puede ser instanciada directamente y que se utiliza como base para otras clases. Su finalidad es definir una estructura común para un conjunto de clases derivadas, pudiendo incluir tanto métodos con implementación como métodos sin implementación.

Un método abstracto es un método que se declara sin cuerpo (sin implementación) y que obliga a las clases hijas a implementarlo. Este tipo de método solo puede existir dentro de una clase abstracta, ya que forma parte de un contrato que las subclases deben cumplir.

No es posible crear instancias de una clase abstracta, ya que está incompleta por definición. Su uso se basa en la herencia: se crean objetos de las clases hijas, que sí implementan todos los métodos necesarios, incluidos los abstractos.

En el ejemplo, Soldado debe declararse como abstract porque contiene un método abstracto atacar. Además, las subclases como Zapador y Artillero están obligadas a implementar dicho método. El modificador abstract se coloca en la declaración de la clase y en la firma del método que no tiene implementación.

abstract class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final en Java impone restricciones que afectan directamente a la herencia y, por tanto, al polimorfismo. Cuando se aplica a una clase, impide que dicha clase pueda ser heredada. Esto significa que no se pueden crear subclases, lo que elimina cualquier posibilidad de polimorfismo basado en esa clase como supertipo.

Cuando final se aplica a un método, se impide que dicho método pueda ser sobrescrito en subclases. En este caso, la clase sí puede heredarse, pero el comportamiento concreto de ese método queda fijo, evitando la ligadura dinámica para ese método concreto. Esto limita el polimorfismo, ya que no se puede redefinir su comportamiento en clases hijas.

La relación con el polimorfismo es que final actúa como una “restricción” al mismo: mientras el polimorfismo busca permitir comportamientos variables a través de la sobreescritura, final bloquea esa variabilidad en los puntos donde se aplica. Por ello, se utiliza cuando se desea garantizar que cierta lógica no sea modificada en jerarquías de herencia.

Un ejemplo de clase final en la API estándar de Java es String. Esta clase no puede ser heredada, lo que asegura que su comportamiento sea seguro, inmutable y consistente. Esto es especialmente importante porque muchas operaciones internas del lenguaje dependen de la fiabilidad de String, como la gestión de literales y la seguridad de la memoria.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java son un tipo de referencia que define un conjunto de métodos que una clase debe implementar, pero sin proporcionar su implementación (al menos en su forma más básica). Su finalidad es establecer un contrato: cualquier clase que implemente una interfaz se compromete a ofrecer el comportamiento definido por sus métodos.

Aunque tienen cierta similitud con las clases abstractas, no son equivalentes. Una clase abstracta puede contener tanto métodos con implementación como métodos abstractos, además de atributos de estado. En cambio, una interfaz tradicional no contiene estado de instancia (salvo constantes) y se centra en definir comportamiento obligatorio. Además, una clase solo puede heredar de una clase abstracta, pero puede implementar múltiples interfaces.

Una clase sí puede implementar más de una interfaz, lo cual es una de las principales ventajas de este mecanismo en Java. Esto permite simular cierta forma de herencia múltiple en cuanto a comportamiento, evitando los problemas de ambigüedad que presenta la herencia múltiple de clases.

En términos de polimorfismo, las interfaces permiten tratar objetos de distintas clases de forma uniforme siempre que compartan un mismo contrato. Por ejemplo, una variable de tipo interfaz puede referenciar cualquier objeto que la implemente, y al invocar sus métodos se ejecutará la implementación concreta correspondiente a cada clase.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

En este diseño, se define una clase abstracta Punto que representa la idea general de un punto, pero sin concretar cómo se calcula la distancia. Para poder trabajar con puntos en 2D y 3D, se crean dos subclases (Punto2D y Punto3D) que implementan de forma específica el método calcularDistanciaA. Este método se declara como abstracto en la clase base para obligar a cada subtipo a definir su propia lógica de cálculo.

Dado que se quiere garantizar que la distancia se calcule únicamente entre puntos del mismo tipo, se emplea instanceof para comprobar la compatibilidad en tiempo de ejecución, junto con downcasting para convertir la referencia de tipo Punto al tipo concreto correspondiente. Esto permite acceder a los atributos específicos (por ejemplo, z en 3D) y evitar errores de mezcla entre dimensiones diferentes.

La clase Linea recibe objetos de tipo Punto sin conocer su implementación concreta, lo que permite aplicar polimorfismo. Al calcular su longitud, simplemente delega en el método calcularDistanciaA, que se resolverá dinámicamente según el tipo real de los puntos. De esta forma, la línea funciona tanto con puntos 2D como 3D sin modificar su código.

abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (!(p instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }

        Punto2D otro = (Punto2D) p;

        double dx = this.x - otro.x;
        double dy = this.y - otro.y;

        return Math.sqrt(dx * dx + dy * dy);
    }
}


class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (!(p instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }

        Punto3D otro = (Punto3D) p;

        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        double dz = this.z - otro.z;

        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

class Linea {
    private Punto p1;
    private Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.calcularDistanciaA(p2);
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz mediante la palabra clave extends. Esto permite construir jerarquías de contratos, donde una interfaz más específica hereda los métodos de otra más general y puede añadir nuevos métodos adicionales. De este modo, se pueden modelar comportamientos progresivamente más especializados sin necesidad de duplicar definiciones.

En Java sí existe la herencia múltiple de interfaces, lo que significa que una interfaz puede extender varias interfaces al mismo tiempo. Esto es posible porque las interfaces no contienen implementación (en su forma clásica), por lo que no se producen conflictos de ambigüedad como ocurre con la herencia múltiple de clases. Gracias a esto, una clase puede acabar implementando múltiples “tipos de comportamiento” a través de una sola o varias interfaces.

En el ejemplo propuesto, se define primero una interfaz Fichero con la operación básica de lectura. Después, otra interfaz FicheroEscribible extiende Fichero y añade operaciones adicionales como escribir contenido y eliminar el fichero. Esto refleja una especialización progresiva del comportamiento.

interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}