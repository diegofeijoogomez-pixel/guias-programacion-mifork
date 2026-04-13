# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva (subclase) a partir de otra ya existente (superclase), reutilizando su estructura y comportamiento. La relación que se establece suele describirse como “A es-un B” (por ejemplo, un Artillero es-un Soldado), lo que implica que la subclase es una especialización de la superclase. Este enfoque permite organizar el código de forma jerárquica, evitando duplicidades y facilitando la extensión de funcionalidades.

La primera implicación importante es la compatibilidad de tipos. Al existir la relación “es-un”, cualquier objeto de la subclase puede tratarse como si fuera de la superclase. Esto significa que un Artillero o un Zapador pueden almacenarse en una variable de tipo Soldado sin problemas. Esta compatibilidad es clave para construir estructuras genéricas, como arrays o colecciones, que trabajen con el tipo base mientras contienen objetos más específicos.

La segunda implicación es la herencia de estado y comportamiento. La subclase hereda los atributos (estado) y métodos (comportamiento) de la superclase, pudiendo utilizarlos directamente o ampliarlos. En el ejemplo, tanto Artillero como Zapador heredan el atributo nombre y el método saludar(). Además, cada subclase añade su propio estado específico (cohetes o minas) y métodos asociados (getters), extendiendo así la funcionalidad sin modificar la clase base.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 8);

        for (Soldado s : ejercito) {
            s.saludar();  // Compatibilidad de tipos: todos son tratados como Soldado
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase, no se ejecuta un único constructor, sino una cadena de constructores que recorre toda la jerarquía de herencia. En el ejemplo, al instanciar un Artillero o un Zapador, primero se ejecuta el constructor de la clase base (Soldado) y después el de la subclase correspondiente. Este orden es obligatorio porque la subclase necesita que la parte heredada (el estado definido en la superclase) esté correctamente inicializada antes de añadir su propio estado. Por tanto, el orden de ejecución es siempre de la superclase a la subclase.

La palabra clave super dentro de un constructor se utiliza para invocar explícitamente un constructor de la clase base. Esta llamada permite pasar los parámetros necesarios para inicializar los atributos heredados. En el ejemplo, super(nombre) en Artillero y Zapador sirve para inicializar el atributo nombre definido en Soldado. Además, esta llamada debe ser siempre la primera instrucción del constructor de la subclase, ya que Java exige que la inicialización de la parte base ocurra antes que cualquier otra operación.

Si la clase base tiene un constructor sin parámetros (constructor por defecto) y es accesible, el compilador inserta automáticamente una llamada implícita a super(). Sin embargo, si la clase base no dispone de un constructor sin parámetros visible (por ejemplo, solo tiene constructores con argumentos), entonces es obligatorio llamar a super(...) de forma explícita desde la subclase, proporcionando los argumentos necesarios. En caso contrario, el código no compilará, ya que Java no sabrá cómo inicializar la parte heredada del objeto.
## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase forman parte de la instancia en memoria de cualquier objeto de una subclase. Cuando se crea un objeto de, por ejemplo, Artillero, ese objeto contiene internamente tanto los atributos definidos en Soldado (como nombre) como los propios de Artillero (como numCohetes). Desde el punto de vista de memoria, el objeto es una única entidad que incluye todo el estado heredado y el específico de la subclase.

Sin embargo, que esos atributos existan en memoria no implica que puedan ser accedidos directamente desde el código de la subclase. El modificador private restringe el acceso exclusivamente a la clase donde se declara. Por tanto, aunque un Artillero tenga un nombre heredado, no puede referirse a él directamente con this.nombre porque ese atributo pertenece a Soldado y es privado.

En el ejemplo, si se quisiera que Artillero o Zapador pudieran utilizar el nombre, sería necesario que la clase Soldado proporcionase algún mecanismo de acceso controlado, como un método getNombre() o cambiar la visibilidad a protected (aunque esto último tiene implicaciones de diseño). De este modo, se respeta el principio de encapsulación: el estado existe en el objeto, pero su acceso está regulado por la clase que lo define.
## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos en herencia implica una gran mejora en la extensibilidad del código. Al poder tratar todos los objetos de las subclases como si fueran del tipo base (Soldado), es posible escribir código genérico que funcione con cualquier subtipo, incluso con aquellos que aún no existen en el momento de escribir dicho código. Esto reduce la necesidad de modificar partes ya implementadas cuando se añaden nuevas funcionalidades, lo que sigue el principio de “abierto/cerrado” (abierto a extensión, cerrado a modificación).

En este contexto, el código que recorre un array de Soldado y llama a saludar() no necesita conocer los detalles concretos de cada subtipo. Simplemente trabaja con la interfaz común definida en la superclase. Si en el futuro se añade un nuevo tipo de soldado, mientras este herede de Soldado y mantenga el comportamiento esperado, el código existente seguirá funcionando sin cambios.

Por ejemplo, se puede añadir una nueva subclase Medico, que tenga su propio estado (número de botiquines), pero que siga siendo un Soldado. El array puede incluir instancias de este nuevo tipo sin modificar el bucle que pide a todos que saluden:

class Medico extends Soldado {
    private int numBotiquines;

    public Medico(String nombre, int numBotiquines) {
        super(nombre);
        this.numBotiquines = numBotiquines;
    }

    public int getNumBotiquines() {
        return numBotiquines;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 8);
        ejercito[3] = new Medico("Carlos", 2); // Nuevo tipo añadido

        for (Soldado s : ejercito) {
            s.saludar();  // No se modifica este código
        }
    }
}

De esta forma, se demuestra que el sistema es fácilmente extensible: se pueden añadir nuevos tipos sin afectar al código existente que opera sobre la superclase.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, en Java es perfectamente válido tener una referencia del supertipo que apunte a un objeto real de un subtipo. Esto es consecuencia directa de la relación “A es-un B”: un Artillero es un Soldado, por lo que puede almacenarse en una variable de tipo Soldado. Sin embargo, al trabajar con esa referencia, el compilador solo permite acceder a los métodos definidos en el tipo de la referencia (Soldado), aunque el objeto real sea más específico.

No es posible invocar directamente métodos exclusivos del subtipo (como getNumCohetes() de Artillero) usando una referencia de tipo Soldado. Para ello es necesario realizar un downcasting, es decir, convertir explícitamente la referencia del supertipo al subtipo concreto. El proceso contrario, cuando una referencia de subtipo se trata como supertipo, se denomina upcasting, y es implícito en Java (no requiere conversión explícita). El downcasting, en cambio, puede ser peligroso si el objeto real no es del tipo esperado.

Para evitar errores en tiempo de ejecución, se utiliza el operador instanceof, que permite comprobar si un objeto es instancia de una clase concreta antes de hacer el casting. De este modo, se puede asegurar que la conversión es segura. En el siguiente ejemplo, se recorre un array de Soldado y, si el objeto real es un Artillero, se obtiene e imprime su número de cohetes:

Soldado[] ejercito = new Soldado[3];

ejercito[0] = new Artillero("Juan", 5);
ejercito[1] = new Zapador("Luis", 3);
ejercito[2] = new Artillero("Ana", 8);

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;  // Downcasting seguro
        System.out.println("Cohetes: " + a.getNumCohetes());
    }
}

En este código se observa el uso combinado de compatibilidad de tipos (array de Soldado con distintos subtipos), instanceof para comprobación de tipo en tiempo de ejecución, y downcasting para acceder a comportamiento específico del subtipo sin comprometer la seguridad del programa.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido (protected) es un modificador de visibilidad que permite que un atributo o método sea accesible no solo dentro de su propia clase, sino también desde las subclases (aunque estén en otros paquetes) y desde otras clases del mismo paquete. Se sitúa, por tanto, entre private (más restrictivo) y public (más abierto), y resulta útil cuando se desea que las clases hijas puedan reutilizar directamente ciertos elementos internos sin exponerlos completamente al exterior.

En términos de ocultación de información, usar protected implica relajar parcialmente el encapsulamiento. Se sigue evitando el acceso general desde cualquier clase externa, pero se permite que las subclases trabajen directamente con ese estado heredado. Esto puede ser conveniente en jerarquías bien controladas, aunque en muchos casos se prefiere mantener los atributos como private y proporcionar métodos de acceso (getters) para un mayor control.

En Java, se implementa simplemente usando la palabra clave protected en la declaración del atributo o método. En el siguiente ejemplo, el atributo nombre de Soldado pasa a ser protegido, lo que permite que la subclase Zapador lo utilice directamente en su método específico:

class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public void ponerMina() {
        System.out.println(nombre + " está colocando una mina");
    }
}

En este caso, Zapador puede acceder directamente al atributo nombre heredado gracias a que es protected. Esto facilita la reutilización en la subclase, aunque debe usarse con criterio para no comprometer el diseño encapsulado del sistema.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe el concepto de una clase base común de la que derivan (directa o indirectamente) todos los objetos. Esta clase suele proporcionar comportamiento básico compartido, como comparación, representación en texto o gestión genérica. Sin embargo, no es una característica universal: algunos lenguajes no imponen una raíz única o permiten trabajar con tipos que no forman parte de una jerarquía común estricta.

En el caso de Java, sí existe una clase base universal: Object. Todas las clases en Java heredan implícitamente de Object si no se especifica otra superclase. Esto significa que cualquier objeto, independientemente de su tipo, puede tratarse como un Object, lo que permite escribir código muy genérico. Además, Object define métodos fundamentales como toString(), equals() o hashCode(), que están disponibles en todos los objetos.

Esta característica tiene implicaciones importantes. Por ejemplo, se puede almacenar cualquier objeto en una variable de tipo Object o en estructuras genéricas basadas en esta clase. Sin embargo, al hacerlo se pierde acceso directo a los métodos específicos de cada clase, siendo necesario realizar conversiones (casting) para recuperarlos. En resumen, Java sí establece una jerarquía única con Object como raíz, lo que facilita la uniformidad y la reutilización en el lenguaje.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es un mecanismo por el cual una clase puede heredar directamente de más de una clase base. Esto permitiría que una subclase reutilice estado y comportamiento de varias superclases simultáneamente. Aunque resulta potente, también introduce complejidad, especialmente cuando dos clases base definen métodos o atributos con el mismo nombre, lo que genera ambigüedades (el conocido “problema del diamante”).

No todos los lenguajes orientados a objetos soportan herencia múltiple de clases. Algunos, como C++, sí la permiten, pero otros la restringen para evitar los problemas mencionados. En el caso de Java, no existe herencia múltiple de clases: una clase solo puede extender directamente de una única superclase (extends). Esta decisión de diseño simplifica el modelo y evita conflictos en la resolución de métodos.

Sin embargo, Java ofrece una alternativa mediante el uso de interfaces. Una clase puede implementar múltiples interfaces (implements), lo que permite heredar múltiples contratos de comportamiento. Desde Java 8, además, las interfaces pueden incluir métodos con implementación (default), lo que acerca parcialmente este mecanismo a la herencia múltiple, aunque sin heredar estado (atributos de instancia). De este modo, Java logra un equilibrio entre flexibilidad y simplicidad en el diseño de jerarquías.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones son objetos que forman parte de una jerarquía cuya raíz es Throwable. Esto permite definir excepciones personalizadas adaptadas a las necesidades del dominio. Si se desea que una excepción sea no controlada (unchecked), debe heredar de RuntimeException. Estas excepciones no obligan a ser declaradas ni capturadas explícitamente, lo que resulta útil para errores de programación o situaciones que no se espera que el código cliente recupere.

Además, una excepción puede estar compuesta con otros objetos, lo que permite aportar más contexto sobre el error. En este caso, la excepción UsuarioNoEncontradoException puede incluir un objeto Usuario para indicar qué entidad concreta provocó el problema. Esto es especialmente útil para depuración o registro de errores, ya que encapsula información relevante directamente en la excepción.

También es habitual permitir encadenar excepciones mediante una causa subyacente (cause). Para ello, se sobrecargan los constructores y se utiliza el constructor de la superclase que acepta tanto un mensaje como una causa. A continuación se muestra un ejemplo completo:

class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getNombre(), cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

En este ejemplo, la excepción es no controlada al heredar de RuntimeException, contiene una referencia al Usuario que causó el error y ofrece dos constructores: uno básico y otro que permite incluir la causa original. Esto facilita tanto el uso como el diagnóstico de errores en aplicaciones más complejas.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe utilizarse únicamente como mecanismo de reutilización de código porque implica una relación estructural y semántica fuerte entre clases. Cuando una clase hereda de otra, se establece un vínculo “es-un” que afecta no solo al código reutilizado, sino también al diseño global del sistema. Esto hace que las clases queden más acopladas, lo que puede dificultar su evolución y mantenimiento a largo plazo.

El principal problema es que la herencia expone la implementación de la superclase a las subclases, incluso cuando se usan modificadores como protected. Esto puede provocar dependencias no deseadas: cambios en la clase base pueden afectar a todas las subclases de forma indirecta. Además, puede romperse el principio de sustitución de Liskov si la subclase no puede comportarse realmente como la superclase en todos los contextos, lo que genera diseños frágiles.

Por este motivo, en muchos casos se prefiere la composición (“tiene-un” en lugar de “es-un”). La composición permite reutilizar funcionalidad delegando en otros objetos sin establecer una jerarquía rígida. Esto reduce el acoplamiento y mejora la flexibilidad del diseño, ya que los componentes pueden cambiarse o combinarse sin modificar toda la estructura. En resumen, la herencia se reserva para relaciones auténticas de especialización, mientras que la composición se utiliza como estrategia principal de reutilización de código.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se dice que se debe favorecer la composición frente a la herencia porque la composición permite construir sistemas más flexibles y menos acoplados. En lugar de establecer una relación rígida “es-un”, la composición utiliza relaciones “tiene-un”, donde un objeto delega parte de su comportamiento en otros objetos. Esto reduce la dependencia directa entre clases y facilita la modificación o sustitución de componentes sin afectar a toda la jerarquía.

En cambio, la herencia crea un acoplamiento fuerte entre la superclase y las subclases, ya que estas dependen de la implementación interna de la clase base. Aunque la herencia permite reutilización de código, también hace que los cambios en la superclase puedan propagarse de forma inesperada a todas las subclases. Esto puede romper el comportamiento de clases derivadas o introducir errores difíciles de prever, especialmente en jerarquías profundas.

Por el contrario, con la composición se pueden combinar comportamientos en tiempo de ejecución mediante objetos colaboradores. Esto mejora la extensibilidad y sigue principios de diseño como la separación de responsabilidades y el principio abierto/cerrado. En resumen, la composición ofrece mayor flexibilidad, mejor mantenimiento y menor riesgo de efectos secundarios, mientras que la herencia se reserva para relaciones realmente naturales de especialización.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Cuando se afirma que la herencia rompe la encapsulación, se hace referencia a que la subclase queda parcialmente dependiente de la implementación interna de la superclase. Aunque la encapsulación pretende ocultar los detalles internos y exponer solo una interfaz controlada, la herencia permite que esos detalles influyan directamente en el comportamiento de las clases derivadas.

Esto ocurre porque la subclase no solo utiliza la interfaz pública de la superclase, sino que también depende de aspectos como la estructura interna, el uso de protected, o incluso el orden de ejecución de métodos heredados. Si la superclase cambia su implementación interna (por ejemplo, modifica cómo implementa un método), la subclase puede verse afectada aunque la interfaz pública no haya cambiado, lo que indica una pérdida de aislamiento real.

En contraste, con la composición, los objetos interactúan únicamente a través de interfaces bien definidas, sin depender de cómo está implementado internamente el otro objeto. Por eso se dice que la herencia puede debilitar la encapsulación: expone más de lo deseado de la superclase a las subclases y crea un acoplamiento implícito con su implementación.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

En este caso pueden modelarse dos alternativas para reutilizar los datos comunes (dni y nombre): mediante herencia o mediante composición. En ambos enfoques se persigue evitar duplicación de código, pero la forma de estructurar la relación entre clases es distinta.

En la solución por herencia, se establece una relación “es-un”, donde Estudiante y Trabajador son tipos de Persona. Los atributos comunes se colocan en la superclase y las subclases heredan dicho estado y comportamiento. Esto permite reutilización directa, pero también crea un acoplamiento fuerte entre la jerarquía de clases.

class Estudiante extends Persona {
    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}

class Trabajador extends Persona {
    private String empresa;

    public Trabajador(String dni, String nombre, String empresa) {
        super(dni, nombre);
        this.empresa = empresa;
    }
}

En la solución por composición, se define una clase independiente DatosPersonales que agrupa la información común. Tanto Estudiante como Trabajador reciben una instancia de esta clase en su constructor, estableciendo una relación “tiene-un”. Esto reduce el acoplamiento y permite reutilizar DatosPersonales en otros contextos sin depender de una jerarquía fija.

class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }
}

class Trabajador {
    private DatosPersonales datos;
    private String empresa;

    public Trabajador(DatosPersonales datos, String empresa) {
        this.datos = datos;
        this.empresa = empresa;
    }
}

En este segundo enfoque se observa una mayor flexibilidad, ya que DatosPersonales puede reutilizarse de forma independiente y la estructura no queda limitada por una jerarquía de clases.