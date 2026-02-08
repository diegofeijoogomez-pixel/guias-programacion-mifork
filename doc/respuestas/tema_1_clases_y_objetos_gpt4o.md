# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

La abstracción consiste en representar entidades del mundo real o conceptos del problema únicamente con la información relevante, ocultando los detalles innecesarios. Permite centrarse en qué hace un objeto en lugar de cómo lo hace, facilitando la comprensión y el diseño del sistema.

La encapsulación implica agrupar datos y operaciones que trabajan sobre esos datos dentro de una misma unidad, llamada objeto. Además, se controla el acceso a los datos internos, evitando que se modifiquen directamente desde fuera, lo que mejora la seguridad y el mantenimiento del código.

La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando su comportamiento y ampliándolo o modificándolo. De esta forma se evita la duplicación de código y se establecen relaciones jerárquicas entre clases.

El polimorfismo posibilita que un mismo método tenga comportamientos distintos según el objeto que lo implemente. Esto permite escribir código más genérico y flexible, capaz de trabajar con distintos tipos de objetos de manera uniforme.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java es uno de los lenguajes más representativos de la programación orientada a objetos. Está fuertemente basado en clases y objetos, y prácticamente todo el código se estructura siguiendo este paradigma.

C++ es un lenguaje multiparadigma que permite programación orientada a objetos además de programación estructurada. A diferencia de Java, ofrece control manual de memoria y compatibilidad directa con código en C.

Python también soporta programación orientada a objetos, aunque de forma más flexible y menos estricta. Permite trabajar con clases y objetos, pero no obliga a usarlos en todos los casos.

C# es otro lenguaje muy utilizado, especialmente en el entorno .NET. Está diseñado con la orientación a objetos como base y comparte muchos conceptos con Java, como clases, interfaces y herencia.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada es un paradigma que organiza el código en bloques lógicos utilizando estructuras de control como secuencias, condicionales y bucles. Su objetivo principal es evitar saltos incontrolados como el uso de goto, favoreciendo un flujo de ejecución claro y predecible.

En este paradigma, el programa se divide en funciones o procedimientos, cada uno con una tarea concreta. Sin embargo, los datos suelen ser compartidos o globales, lo que puede dificultar el mantenimiento en programas grandes.

La programación modular va un paso más allá, dividiendo el programa en módulos independientes. Cada módulo encapsula un conjunto de funciones relacionadas y define una interfaz clara con el resto del sistema.

Este enfoque mejora la reutilización del código y facilita el desarrollo en equipo, aunque sigue sin integrar completamente datos y comportamiento como lo hace la orientación a objetos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define, en primer lugar, por su estado, que está formado por los valores de sus atributos o variables internas. Estos datos representan la información que caracteriza al objeto en un momento concreto.

En segundo lugar, un objeto se define por su comportamiento, es decir, el conjunto de métodos u operaciones que puede realizar. Estos métodos permiten interactuar con el objeto y modificar su estado de forma controlada.

Por último, un objeto posee una identidad, que lo distingue de otros objetos aunque tengan el mismo estado. Dos objetos pueden tener los mismos valores en sus atributos y, aun así, ser entidades distintas en memoria.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o definición que describe cómo serán los objetos de un determinado tipo. En ella se especifican los atributos que tendrán los objetos y los métodos que podrán ejecutar.

Un objeto no es lo mismo que una clase. El objeto es una entidad concreta creada a partir de una clase, con valores específicos para sus atributos. El proceso de crear un objeto a partir de una clase se denomina instanciación.

Una instancia es, por tanto, un objeto concreto de una clase determinada. Puede haber múltiples instancias de una misma clase, todas independientes entre sí.

No todos los lenguajes orientados a objetos utilizan necesariamente clases. Algunos lenguajes, como JavaScript, se basan en un modelo de prototipos, aunque el concepto de objeto sigue estando presente.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En lenguajes como Java, los objetos se almacenan normalmente en el montón (heap), y las variables que los manejan contienen referencias a esas posiciones de memoria. El programador no controla directamente dónde se ubican ni cuándo se liberan.

No todos los lenguajes gestionan la memoria de la misma forma. En C++, por ejemplo, un objeto puede almacenarse en la pila o en el montón, y el programador es responsable de liberar la memoria cuando deja de ser necesaria.

La recolección de basura es un mecanismo automático que se encarga de liberar la memoria ocupada por objetos que ya no se utilizan. El sistema detecta cuándo un objeto no tiene referencias activas y recupera su memoria sin intervención explícita del programador.

Este enfoque reduce errores como fugas de memoria o accesos inválidos, a costa de perder control directo sobre la gestión de memoria.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función asociada a una clase u objeto que define un comportamiento. A diferencia de las funciones en C, los métodos operan normalmente sobre los atributos del objeto al que pertenecen.

Los métodos permiten encapsular la lógica relacionada con un objeto y proporcionan una interfaz clara para interactuar con él. Desde fuera, solo se conoce qué hace el método, no cómo está implementado internamente.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre pero con diferentes parámetros. El compilador decide cuál usar en función del número y tipo de argumentos proporcionados.

Esto permite ofrecer distintas formas de realizar una misma operación, mejorando la legibilidad y la flexibilidad del código.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

class Prueba {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método main. Este método es el primero que se ejecuta cuando se lanza una aplicación y debe tener una firma concreta para que la máquina virtual pueda localizarlo.

La palabra clave static indica que un método o atributo pertenece a la clase y no a una instancia concreta. Esto permite usarlo sin necesidad de crear un objeto previamente.

static no se emplea únicamente en el método main. Puede utilizarse para métodos utilitarios, constantes o atributos compartidos por todas las instancias de una clase.

Cuando se combina static con final, se define una constante de clase. Esto se utiliza para valores fijos que no deben cambiar y que son comunes a todos los objetos

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Java se compila utilizando el comando javac, que transforma el código fuente (.java) en archivos .class. Posteriormente, el programa se ejecuta con el comando java, indicando la clase que contiene el método main.

Java es un lenguaje compilado e interpretado. No se compila directamente a código máquina, sino a un formato intermedio llamado byte-code.

La máquina virtual de Java (JVM) es la encargada de ejecutar ese byte-code. Actúa como una capa intermedia entre el programa y el sistema operativo.

Los archivos .class contienen el byte-code, que es independiente de la plataforma. Esto permite que el mismo programa Java se ejecute en distintos sistemas sin recompilar.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

new es el operador que se utiliza para crear un nuevo objeto en memoria a partir de una clase. Reserva espacio y devuelve una referencia al objeto creado.

Un constructor es un método especial que se ejecuta automáticamente al crear un objeto. Su función principal es inicializar los atributos del objeto.

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es una referencia al objeto actual sobre el que se está ejecutando un método. Permite acceder a los atributos y métodos de ese objeto de forma explícita.

Se utiliza especialmente cuando existe ambigüedad entre parámetros y atributos con el mismo nombre. Gracias a this, se distingue claramente el atributo del objeto.

No todos los lenguajes usan el mismo nombre. En C++ se utiliza this, en Python se emplea self, aunque el concepto es equivalente.

double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, los parámetros se pasan siempre por valor. En el caso de objetos, lo que se copia es la referencia, no el objeto en sí. Por ello, modificar los atributos del objeto dentro del método afecta al objeto original.

Sin embargo, si dentro del método se reasigna la referencia para que apunte a otro objeto, ese cambio no se refleja fuera del método.

En el caso de tipos primitivos como int, se copia directamente el valor. Cualquier modificación realizada dentro del método no afecta a la variable original.

Esto implica una diferencia clara entre el comportamiento de objetos y tipos primitivos al pasarlos como parámetros.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

toString() es un método definido en la clase base Object de Java. Su objetivo es devolver una representación en forma de texto del objeto.

Este método se utiliza automáticamente en muchas situaciones, como al imprimir un objeto por consola. Sobrescribirlo permite definir una salida más informativa.

Conceptos similares existen en otros lenguajes, como __str__ en Python o métodos de conversión a cadena en C++.

@Override
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Una clase puede verse como una evolución del struct en C, ya que ambos agrupan datos relacionados. Sin embargo, el struct solo contiene datos y no comportamiento asociado.

A un struct le falta la encapsulación, el control de acceso y la posibilidad de definir métodos que operen directamente sobre los datos.

Además, las clases permiten herencia, polimorfismo y constructores, conceptos inexistentes en los struct tradicionales de C.

Por tanto, aunque conceptualmente son similares, una clase ofrece muchas más posibilidades de diseño y abstracción.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double distanciaAlOrigen(Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}


En este caso, el struct contiene únicamente los datos, y la función recibe explícitamente el punto como parámetro. El papel de this se sustituye por ese parámetro.

La relación entre datos y funciones es más débil, ya que no existe un vínculo directo entre ambos. Esto obliga al programador a mantener manualmente la coherencia.

La orientación a objetos integra datos y comportamiento en una única entidad, reduciendo errores y mejorando la claridad del diseño.
