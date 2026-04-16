<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
1. Brevemente, ¿qué es el "polimorfismo" y para qué sirve en 
programación orientada a objetos? ¿qué es la "sobreescritura" de 
métodos? 
El polimorfismo es la capacidad de que una misma referencia de un tipo general (por 
ejemplo, Soldado) pueda apuntar a objetos de distintos subtipos (Zapador, Artillero) y 
que, al invocar un método, se ejecute la versión correspondiente al tipo real del objeto. 
Sirve para: 
• escribir código más general y extensible, 
• permitir que nuevas subclases funcionen sin modificar el código existente, 
• habilitar comportamientos especializados según el tipo real del objeto. 
La sobreescritura (overriding) consiste en que una subclase redefine un método 
heredado, sustituyendo su comportamiento. 
2. ¿En qué consiste la "ligadura dinámica" o "enlace tardío"? ¿qué 
relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente 
al programar o depende esto del lenguaje? Compara C++ y Java. 
Indicalo después también para Python. 
La ligadura dinámica es el mecanismo por el cual la selección del método que se 
ejecuta se decide en tiempo de ejecución, según el tipo real del objeto, no según el 
tipo de la referencia. 
Relación con el polimorfismo: 
• es el mecanismo que hace posible el polimorfismo de subtipo. 
Dependencia del lenguaje: 
• Java: la ligadura dinámica es automática para métodos no estáticos. No hay 
que indicar nada. 
• C++: solo hay ligadura dinámica si el método se declara como virtual. Si no, la 
ligadura es estática. 
• Python: todo es dinámico por defecto; la ligadura dinámica es automática. 
3. Ejemplo sencillo en Java con Soldado, Zapador y Artillero, donde 
Zapador sobreescribe saludar. 
java 
class Soldado { 
    public void saludar() { 
        System.out.println("Soy un soldado"); 
    } 
} 
 
class Zapador extends Soldado { 
    @Override 
    public void saludar() { 
        System.out.println("Soy un zapador"); 
    } 
} 
 
class Artillero extends Soldado { 
} 
 
public class Main { 
    public static void main(String[] args) { 
        Soldado[] escuadra = { 
            new Zapador(), 
            new Artillero(), 
            new Zapador() 
        }; 
 
        for (Soldado s : escuadra) { 
            s.saludar(); // polimorfismo 
        } 
    } 
} 
 
4. Si sobreescribo un método, ¿puedo invocar el método base para 
trabajar a partir de su resultado? ¿Qué palabra clave se usa? 
Sí, puedes invocar el método de la superclase mediante la palabra clave super. 
Ejemplo: 
java 
class Soldado { 
    public void saludar() { 
        System.out.println("Soy un soldado"); 
    } 
} 
 
class Zapador extends Soldado { 
    @Override 
    public void saludar() { 
        super.saludar(); // invoca al método base 
        System.out.println("ZAPADOR A SUS ORDENES"); 
    } 
} 
 
5. Restricciones al sobreescribir métodos en Java. Diferencia entre 
overriding y overloading. Uso de @Override. 
Restricciones: 
• el nombre del método debe ser el mismo, 
• los parámetros deben ser exactamente los mismos, 
• el tipo de retorno debe ser el mismo o un subtipo (retorno covariante), 
• la visibilidad no puede ser más restrictiva, 
• no se pueden lanzar excepciones más generales que las del método base (para 
checked exceptions). 
Diferencias: 
• overriding: redefinir un método heredado con la misma firma. 
• overloading: definir varios métodos con el mismo nombre pero distinta lista de 
parámetros. 
@Override sirve para: 
• indicar explícitamente que se está sobreescribiendo un método, 
• permitir que el compilador detecte errores (por ejemplo, si el método base 
cambia su firma). 
6. ¿Se emplea el polimorfismo desde el principio en Java? 
¿Sobrescribir toString o equals es polimorfismo? 
Sí. Cada vez que sobrescribes toString, equals, hashCode o cualquier método 
heredado de Object, estás usando polimorfismo, porque la versión que se ejecuta 
depende del tipo real del objeto. 
7. ¿Qué es una clase abstracta? ¿Qué es un método abstracto? ¿Puedo 
crear instancias? Ejemplo con Soldado y atacar. 
Una clase abstracta es una clase que no puede instanciarse y que puede contener 
métodos abstractos. 
Un método abstracto es un método sin implementación, que obliga a las subclases a 
implementarlo. 
No, no se pueden crear instancias de una clase abstracta. 
Ejemplo: 
java 
abstract class Soldado { 
    public void saludar() { 
        System.out.println("Soy un soldado"); 
    } 
 
    public abstract void atacar(); // método abstracto 
} 
 
class Zapador extends Soldado { 
    @Override 
    public void atacar() { 
        System.out.println("Coloco una mina"); 
    } 
} 
 
class Artillero extends Soldado { 
    @Override 
    public void atacar() { 
        System.out.println("Lanzo un cohete"); 
    } 
} 
La palabra clave abstract se pone en la clase y en el método. 
8. Efecto de final en métodos y clases. Relación con polimorfismo. 
Ejemplo de la API. 
• Un método final no puede ser sobreescrito. 
• Una clase final no puede tener subclases. 
Relación con polimorfismo: 
• final bloquea la posibilidad de polimorfismo por sobrescritura. 
Ejemplo de clase final en Java: 
• java.lang.String es final. 
9. ¿Qué son las interfaces en Java? ¿Son como clases abstractas? 
¿Una clase puede implementar más de una? 
Una interfaz es un tipo que declara métodos sin implementación (hasta Java 7) o con 
implementación por defecto (desde Java 8), pero no tiene estado obligatorio. 
Son similares a clases abstractas, pero: 
• no pueden tener atributos de instancia, 
• permiten herencia múltiple de interfaces. 
Una clase puede implementar todas las interfaces que quiera. 
10. Ejemplo con Punto 2D y 3D, método abstracto calcularDistanciaA, 
uso de instanceof y Linea. 
java 
abstract class Punto { 
public abstract double calcularDistanciaA(Punto otro); 
} 
class Punto2D extends Punto { 
private double x, y; 
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 
 
    @Override 
    public double calcularDistanciaA(Punto otro) { 
        if (!(otro instanceof Punto2D)) { 
            throw new IllegalArgumentException("Tipo incompatible"); 
        } 
        Punto2D p = (Punto2D) otro; 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 
2)); 
    } 
} 
 
class Punto3D extends Punto { 
    private double x, y, z; 
 
    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 
 
    @Override 
    public double calcularDistanciaA(Punto otro) { 
        if (!(otro instanceof Punto3D)) { 
            throw new IllegalArgumentException("Tipo incompatible"); 
        } 
        Punto3D p = (Punto3D) otro; 
        return Math.sqrt(Math.pow(x - p.x, 2) + 
                         Math.pow(y - p.y, 2) + 
                         Math.pow(z - p.z, 2)); 
    } 
} 
 
class Linea { 
    private Punto a, b; 
 
    public Linea(Punto a, Punto b) { 
        this.a = a; 
        this.b = b; 
    } 
 
public double longitud() { 
return a.calcularDistanciaA(b); 
} 
} 
11. ¿Qué es la "herencia de interfaces" en Java? ¿Existe herencia 
múltiple de interfaces? Ejemplo. 
La herencia de interfaces consiste en que una interfaz puede extender a otra, 
heredando sus métodos. 
Sí, existe herencia múltiple de interfaces: una interfaz puede extender varias 
interfaces. 
Ejemplo: 
java 
interface Fichero { 
String leer(); 
} 
interface FicheroEscribible extends Fichero { 
void escribir(String contenido); 
void eliminar(); 
} 