<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

1. ¿Cuáles son las cuatro características básicas de la 
programación orientada a objetos? Describe 
brevemente cada una 
Respuesta: 
• Abstracción: Representa solo lo esencial de un objeto, ocultando detalles 
innecesarios. 
• Encapsulamiento: Agrupa datos y métodos y controla su acceso. 
• Herencia: Permite crear clases basadas en otras, reutilizando código. 
• Polimorfismo: Un mismo método puede comportarse de forma distinta según 
el objeto que lo invoque. 
2. Cita cuatro lenguajes populares que permitan la 
programación orientada a objetos 
Respuesta: Java, C++, Python y C#. 
3. Los paradigmas anteriores a la POO, ¿Qué es la 
programación estructurada? y, todavía mejor, ¿Qué es 
la programación modular? 
Respuesta: 
• Programación estructurada: Divide el programa en bloques lógicos usando 
secuencia, selección e iteración. Evita goto. 
• Programación modular: Divide el programa en módulos independientes y 
reutilizables, cada uno con una responsabilidad clara. 
4. ¿Qué tres elementos definen a un objeto en 
programación orientada a objetos? 
Respuesta: Atributos (estado), métodos (comportamiento) e identidad (cada objeto es 
único). 
5. ¿Qué es una clase? ¿Es lo mismo que un objeto? 
¿Qué es una instancia? ¿Todos los lenguajes 
orientados a objetos manejan el concepto de clase? 
Respuesta: 
• Clase: Plantilla que define atributos y métodos. 
• Objeto: Entidad creada a partir de una clase. 
• Instancia: Sinónimo de objeto creado. 
• ¿Todos los lenguajes usan clases? No. JavaScript o Python permiten objetos 
sin necesidad estricta de clases. 
6. ¿Dónde se almacenan en memoria los objetos? ¿Es 
igual en todos los lenguajes? ¿Qué es la recolección de 
basura? 
Respuesta: 
• Depende del lenguaje: 
o Java → objetos en heap. 
o C++ → pueden estar en stack o heap. 
• Recolección de basura: mecanismo automático que libera memoria de objetos 
no usados. 
7. ¿Qué es un método? ¿Qué es la sobrecarga de 
métodos? 
Respuesta: 
• Método: función dentro de una clase. 
• Sobrecarga: varios métodos con el mismo nombre pero distinta lista de 
parámetros. 
8. Ejemplo mínimo de clase en Java, que se llame 
Punto… 
Respuesta: 
java 
public class Punto { 
    int x; 
    int y; 
 
    double calculaDistanciaAOrigen() { 
        return Math.sqrt(x * x + y * y); 
    } 
} 
 
class Ejemplo { 
    public static void main(String[] args) { 
        Punto p = new Punto(); 
        p.x = 3; 
        p.y = 4; 
        System.out.println(p.calculaDistanciaAOrigen()); 
    } 
} 
 
9. ¿Cuál es el punto de entrada en un programa en 
Java? ¿Qué es static y para qué vale? ¿Sólo se emplea 
para ese método main? ¿Para qué se combina con 
final? 
Respuesta: 
• El punto de entrada es: 
java 
public static void main(String[] args) 
 
• static: pertenece a la clase, no a un objeto. 
• Se usa también para constantes y métodos utilitarios. 
• static + final: define constantes de clase. 
10. ¿Cómo podemos compilar el programa y ejecutarlo 
desde línea de comandos? ¿Java es compilado? ¿Qué 
es la máquina virtual? ¿Qué es el byte-code y los 
ficheros .class? 
Respuesta: 
• Compilar: 
Código 
javac Archivo.java 
• Ejecutar: 
Código 
java NombreClase 
• Java es compilado a bytecode, no a código máquina. 
• La JVM ejecuta ese bytecode. 
• Los .class contienen el bytecode generado por javac. 
11. En el código anterior de la clase Punto ¿Qué es 
new? ¿Qué es un constructor? Pon un ejemplo de 
constructor en una clase Empleado… 
Respuesta: 
• new: crea un objeto en memoria. 
• Constructor: método especial que inicializa el objeto. 
Ejemplo: 
java 
public class Empleado { 
String dni; 
String nombre; 
String apellidos; 
    public Empleado(String dni, String nombre, String apellidos) { 
        this.dni = dni; 
        this.nombre = nombre; 
        this.apellidos = apellidos; 
    } 
} 
 
12. ¿Qué es la referencia this? ¿Se llama igual en todos 
los lenguajes? Pon un ejemplo… 
Respuesta: 
• this: referencia al objeto actual. 
• No se llama igual en todos los lenguajes (Python usa self). 
Ejemplo: 
java 
double calculaDistanciaAOrigen() { 
    return Math.sqrt(this.x * this.x + this.y * this.y); 
} 
 
13. Añade ahora otro nuevo método distanciaA… 
Respuesta: 
java 
double distanciaA(Punto otro) { 
    int dx = this.x - otro.x; 
    int dy = this.y - otro.y; 
    return Math.sqrt(dx * dx + dy * dy); 
} 
 
14. El paso del Punto como parámetro… por copia o por 
referencia? ¿Qué ocurre con un int? 
Respuesta: 
• En Java, los objetos se pasan por valor, pero el valor es una referencia. → Si 
modificas atributos del Punto dentro del método, cambian fuera. 
• Los tipos primitivos (int, double…) se pasan por copia. → Si modificas el int 
dentro del método, fuera no cambia. 
15. ¿Qué es el método toString() en Java? ¿Existe en 
otros lenguajes? Pon un ejemplo… 
Respuesta: 
• Convierte el objeto a una cadena legible. 
• Existe en otros lenguajes (Python: __str__, C#: ToString()). 
Ejemplo: 
java 
@Override 
public String toString() { 
return "(" + x + ", " + y + ")"; 
} 
16. Reflexiona: ¿una clase es como un struct en C? 
¿Qué le falta al struct…? 
Respuesta: Un struct solo agrupa datos. Le faltan: métodos, encapsulamiento, 
herencia, polimorfismo y comportamiento asociado. 
17. ¿Cómo se podría “emular” con struct en C la clase 
Punto…? ¿Qué ha pasado con this? 
Respuesta: 
c 
#include <math.h> 
typedef struct { 
int x; 
int y; 
} Punto; 
double distanciaAOrigen(Punto p) { 
return sqrt(p.x * p.x + p.y * p.y); 
} 
• this desaparece, porque en C no existe. 
• Se pasa el struct como parámetro.