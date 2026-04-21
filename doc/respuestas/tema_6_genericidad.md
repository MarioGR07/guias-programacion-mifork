<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
TEMA 6. Genericidad
1. Empleando void\ en C o Object en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.*
En C (void\*)
c
typedef struct {
    void* datos[100];
    int size;
} Lista;

void add(Lista* l, void* elemento) {
    l->datos[l->size++] = elemento;
}
En Java (Object)
java
class Lista {
    private Object[] datos = new Object[100];
    private int size = 0;

    public void add(Object o) {
        datos[size++] = o;
    }

    public Object get(int i) {
        return datos[i];
    }
}
2. Brevemente, ¿Qué significa la programación genérica? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?
La programación genérica permite escribir código que funciona con distintos tipos, sin duplicarlo y manteniendo seguridad de tipos.

El ejemplo anterior no es programación genérica real, porque usa void* u Object, lo que pierde el tipo concreto y obliga a hacer downcasting. Es solo una simulación primitiva.

3. Indica los problemas respecto al chequeo de tipos, de emplear void\ o Object cuando se crean estructuras de datos genéricas.*
No hay chequeo de tipos en compilación.

Se permite insertar cualquier cosa, incluso por error.

Requiere downcasting, que puede fallar en tiempo de ejecución.

Mayor probabilidad de errores y menor legibilidad.

4. Vamos entonces con mecanismos de mejora de la programación genérica. ¿Qué son los parámetros de tipo?
Son variables de tipo que se declaran entre < > y permiten que una clase o método trabaje con un tipo concreto sin perder seguridad.

Ejemplo:

java
class Caja<T> {
    private T valor;
}
T es un parámetro de tipo.

5. Ejemplo de programación genérica en Java y C++ con listas que solo admiten String.
Java
java
List<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}
C++
cpp
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("Hola");
v.push_back("Mundo");

for (const std::string& s : v) {
    std::cout << s << std::endl;
}
Ambos garantizan que solo se aceptan String.

6. ¿Qué hace el compilador al instanciar una clase genérica? ¿Hace lo mismo C++ y Java? ¿Qué es type erasure y qué es instanciación de plantillas?
Java (type erasure)  
El compilador elimina los tipos genéricos y los sustituye por Object (o el bound).
No genera código nuevo por cada tipo.

C++ (template instantiation)  
El compilador genera una copia del código para cada tipo concreto.
Es expansión en tiempo de compilación.

7. Clase Par<T,U> en Java y ejemplo de uso.
java
class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() { return primero; }
    public B getSegundo() { return segundo; }
}
Ejemplo de uso:

java
public Par<Double, Double> calcularStats(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;

    double var = 0;
    for (double d : datos) var += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(var / datos.length);

    return new Par<>(media, desviacion);
}
8. Método genérico seleccionaUno y comparación con Object.
Con Object (malo)
java
public Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
Problemas:

No garantiza que a y b sean del mismo tipo.

Requiere downcasting al usarlo.

Con genéricos (bueno)
java
public <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
Ventajas:

Ambos parámetros deben ser del mismo tipo T.

No hay downcasting.

Seguridad en compilación.

9. Restricciones en parámetros de tipo. Punto con Number y Punto<T extends Number>.
Solución 1: usando Number
java
class Punto {
    private Number x, y;

    public Punto(Number x, Number y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
Solución 2: usando generics
java
class Punto<T extends Number> {
    private T x, y;

    public Punto(T x, T y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
Tipo final tras compilación (type erasure)
T se convierte en Number.

10. Reflexión sobre el chequeo de tipos.
Con la versión sin genéricos (Number), puedes crear:

java
new Punto(3, 4.5); // permitido
No hay restricción entre coordenadas.

Con generics:

java
new Punto<Integer>(3, 4.5); // NO permitido
Ambos deben ser del mismo tipo T.

getX():

Sin generics → devuelve Number

Con generics → devuelve T (tipo exacto)

11. Reescritura del ejemplo Punto2D/Punto3D usando generics para evitar instanceof.
java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2)
                       + Math.pow(z - p.z, 2));
    }
}
Ya no hay instanceof ni downcasting.

12. ¿List<String> es subtipo de List<Object>? ¿Y String[] de Object[]? Covarianza, contravarianza e invariancia.
List<String> NO es subtipo de List<Object>  
→ Los genéricos en Java son invariantes.

String[] SÍ es subtipo de Object[]  
→ Los arrays son covariantes, pero esto causa errores en tiempo de ejecución:

java
Object[] arr = new String[10];
arr[0] = 3; // ERROR en tiempo de ejecución
Definiciones:
Covariante: A es subtipo de B ⇒ F<A> es subtipo de F<B>.

Contravariante: A es subtipo de B ⇒ F<B> es subtipo de F<A>.

Invariante: No hay relación de subtipado.

13. Wildcards en Java. ? extends y ? super.
? significa “algún tipo desconocido”.

? extends T → covariante, puedes leer, no puedes escribir.

? super T → contravariante, puedes escribir T, no puedes leer con tipo exacto.

(i) Lista de números y suma (? extends Number)
java
double suma(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) s += n.doubleValue();
    return s;
}
(ii) Añadir enteros (? super Integer)
java
void insertarEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}