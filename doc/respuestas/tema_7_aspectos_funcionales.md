<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado aMayusculas e invócala con el puntero.
Un puntero a función en C es una variable que almacena la dirección de una función, permitiendo invocarla a través del puntero.

c
#include <stdio.h>
#include <ctype.h>

char *a_mayusculas(char *s) {
    for (char *p = s; *p != '\0'; p++) {
        *p = (char) toupper((unsigned char)*p);
    }
    return s;
}

int main(void) {
    char texto[] = "hola mundo";

    // puntero a función: recibe char* y devuelve char*
    char *(*aMayusculas)(char *);
    aMayusculas = a_mayusculas;

    printf("%s\n", aMayusculas(texto));  // invocación vía puntero
    return 0;
}
2. ¿Qué es una función lambda en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local aMayusculas para apuntar a la función lambda. Por simplicidad, en Java, emplea Function<String, String> para el tipo de la referencia a la función lambda.
Una función lambda es una función anónima (sin nombre) que se puede tratar como un valor: se puede asignar a variables, pasar como parámetro, devolver, etc.

JavaScript:

js
// función lambda (arrow function) que pasa a mayúsculas
const aMayusculas = (s) => s.toUpperCase();

console.log(aMayusculas("hola mundo"));
Java:

java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
3. ¿Qué es el paradigma funcional? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?
El paradigma funcional se basa en tratar la computación como evaluación de funciones puras, evitando estado mutable y efectos secundarios, y favoreciendo composición de funciones y expresiones.

Java 8 se considera multiparadigma porque combina orientación a objetos con características funcionales (lambdas, streams, funciones como parámetros, interfaces funcionales).

Que las funciones sean “ciudadanos de primera clase” significa que se pueden usar como cualquier otro valor: asignarlas a variables, pasarlas como argumentos, devolverlas de funciones, almacenarlas en estructuras de datos, etc.

4. Explica la sintaxis básica de una función lambda en Java.
La forma general es:

java
(parametros) -> cuerpo
Casos típicos:

Un parámetro, expresión:

java
x -> x * x
Varios parámetros:

java
(a, b) -> a + b
Con tipos explícitos:

java
(String s) -> s.toUpperCase()
Con bloque y return:

java
(String s) -> {
    String t = s.trim();
    return t.toUpperCase();
}
El tipo de la lambda viene dado por el contexto, es decir, por la interfaz funcional a la que se asigna.

5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado transformar, que reciba un String como parámetro y luego una función transformadora como lo es aMayúsculas y la invoque desde dentro.
JavaScript:

js
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = s => s.toUpperCase();

console.log(transformar("hola mundo", aMayusculas));
Java:

java
import java.util.function.Function;

public class Main {
    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        System.out.println(transformar("hola mundo", aMayusculas));
    }
}
6. Ahora, invoca transformar, con una nueva función lambda directamente en la llamada a transformar, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.
JavaScript:

js
function transformar(texto, transformador) {
    return transformador(texto);
}

console.log(
    transformar("hola", s => s.split("").reverse().join(""))
);
Java:

java
import java.util.function.Function;

public class Main {
    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        System.out.println(
            transformar("hola", s -> new StringBuilder(s).reverse().toString())
        );
    }
}
7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.
Un closure es una función que “captura” variables de su entorno léxico, pudiendo usarlas incluso cuando el contexto original ya no está activo. En Java, las lambdas pueden capturar variables locales que sean efectivamente finales.

java
import java.util.function.Function;

public class Main {
    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = "!!!";  // variable local capturada (efectivamente final)

        Function<String, String> concatenarSufijo = s -> s + sufijo;

        System.out.println(transformar("hola", concatenarSufijo));
        System.out.println(transformar("adios", s -> s + sufijo));
    }
}
La lambda forma un closure sobre la variable sufijo.

8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?
Algunas diferencias clave:

En C, los punteros a función no capturan entorno: no hay closures; solo apuntan a código.

Las lambdas (en lenguajes como Java, JS) pueden capturar variables del contexto, formando closures.

En C, el tipo de un puntero a función es puramente de firma; en Java, la lambda se asocia a una interfaz funcional concreta.

Las lambdas suelen integrarse con el sistema de tipos de alto nivel (genéricos, interfaces funcionales, inferencia de tipos), mientras que los punteros a función son más “bajos niveles”.

9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa Function<Double, Double> para su tipo. La función crearDescuento(porcentaje), recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.
java
import java.util.function.Function;

public class Main {

    static Function<Double, Double> crearDescuento(double porcentaje) {
        // porcentaje se captura en la lambda (closure)
        return cantidad -> cantidad * (1 - porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}
La closure aquí es que cada lambda devuelta por crearDescuento captura el valor de porcentaje que se le pasó en su creación, y lo usa después al aplicar el descuento.

10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como interfaz funcional. ¿Qué es una interfaz funcional? ¿Qué requisitos tiene?
Una interfaz funcional es una interfaz que declara exactamente un único método abstracto. Ese método abstracto define la “forma” de la función (parámetros y tipo de retorno) y es el objetivo de las lambdas.

Requisitos:

Debe tener un único método abstracto.

Puede tener métodos default y static adicionales.

Suele anotarse con @FunctionalInterface para que el compilador verifique que cumple la condición.

11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale Transformador, que define una función que convierte una cadena de texto (String) en otra (String).
java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}
Ejemplo de uso:

java
public class Main {
    static String aplicar(String texto, Transformador t) {
        return t.transformar(texto);
    }

    public static void main(String[] args) {
        Transformador aMayusculas = s -> s.toUpperCase();
        System.out.println(aplicar("hola", aMayusculas));
    }
}
12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un Transformador de un tipo en otro. Pon un ejemplo de un transformador que redondea un Double en un Integer.
java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
Ejemplo de transformador que redondea un Double a Integer:

java
public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            d -> (int) Math.round(d);

        System.out.println(redondear.transformar(3.6)); // 4
    }
}
13. Transformador, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es Function<T, R>. Muestra las interfaces funcionales predefinidas que hay en Java.
En java.util.function hay varias interfaces funcionales predefinidas, entre otras:

Function<T, R>: T -> R

BiFunction<T, U, R>: (T, U) -> R

Consumer<T>: T -> void

BiConsumer<T, U>

Supplier<T>: () -> T

Predicate<T>: T -> boolean

BiPredicate<T, U>

UnaryOperator<T>: T -> T

BinaryOperator<T>: (T, T) -> T

Versiones primitivas: IntFunction, IntConsumer, IntPredicate, etc.

14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el List.forEach, como versión funcional del bucle for. Emplea el forEach para recorrer una lista de Integer y que muestre un mensaje si el entero es positivo.
java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-2, 0, 3, 5, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
15. Repasando el tema de genericidad, fíjate en la firma de forEach, ¿por qué se usa Consumer<? super T> y no Consumer<T>? Explica qué significa PECS, y explícalo para el caso de mejorar el ejemplo del método transformar la hora de definir el tipo de la función transformadora.
La firma de forEach es:

java
void forEach(Consumer<? super T> action)
Se usa ? super T para permitir consumidores de tipos más generales que T. Esto sigue la regla PECS: “Producer Extends, Consumer Super”.

Producer Extends: cuando una estructura produce valores de tipo T, se usa ? extends T.

Consumer Super: cuando una estructura consume valores de tipo T, se usa ? super T.

En un método transformar, si el transformador consume un String y produce un String, podríamos escribir:

java
static <T> T transformar(
        T valor,
        java.util.function.Function<? super T, ? extends T> transformador) {
    return transformador.apply(valor);
}
Aquí:

? super T indica que el transformador puede aceptar T o cualquier supertipo de T como entrada.

? extends T indica que el resultado puede ser T o subtipo de T.

16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase Persona con un método saludar. En el código principal, crea una Persona con un nombre, y obtén una referencia a su método saludar en una variable local. Invoca saludar con esa referencia a su método saludar.
JavaScript:

js
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const p = new Persona("Mario");

// referencia al método de instancia
const refSaludar = p.saludar.bind(p); // bind para fijar this

refSaludar();
Java:

java
public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Mario");

        Consumer<Persona> refSaludar = Persona::saludar;

        refSaludar.accept(p);
    }
}
Si queremos una referencia al método de una instancia concreta:

java
Runnable refSaludarInstancia = p::saludar;
refSaludarInstancia.run();
17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.
Tipos principales:

Referencia a método estático: Clase::metodoEstatico

Referencia a constructor: Clase::new

Referencia a método de instancia de una instancia concreta: instancia::metodo

Referencia a método de instancia de cualquier instancia de un tipo: Clase::metodoInstancia

Ejemplos:

java
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.Consumer;

class Util {
    static int parse(String s) {
        return Integer.parseInt(s);
    }
}

class Persona {
    String nombre;
    Persona(String nombre) { this.nombre = nombre; }
    void saludar() { System.out.println("Hola, soy " + nombre); }
}

public class Main {
    public static void main(String[] args) {
        // 1. Método estático
        Function<String, Integer> f1 = Util::parse;

        // 2. Constructor
        Function<String, Persona> f2 = Persona::new;

        // 3. Método de instancia de una instancia concreta
        Persona p = new Persona("Mario");
        Runnable f3 = p::saludar;

        // 4. Método de instancia sobre cualquier instancia
        Consumer<Persona> f4 = Persona::saludar;

        System.out.println(f1.apply("123"));
        Persona p2 = f2.apply("Ana");
        f3.run();
        f4.accept(p2);
    }
}
18. Otro ejemplo expresivo. Ordena una lista de Persona, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de Persona con Collections.sort, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando Comparator.
java
import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Mario", 30),
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Bea", 25)
        );

        // Versión 1: comparador manual
        Collections.sort(personas, (p1, p2) -> {
            int cmpEdad = Integer.compare(p1.edad, p2.edad);
            if (cmpEdad != 0) return cmpEdad;
            return p1.nombre.compareTo(p2.nombre);
        });
        System.out.println("Orden manual: " + personas);

        // Versión 2: usando Comparator
        personas = Arrays.asList(
            new Persona("Mario", 30),
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(
            personas,
            Comparator.comparingInt((Persona p) -> p.edad)
                      .thenComparing(p -> p.nombre)
        );
        System.out.println("Orden con Comparator: " + personas);
    }
