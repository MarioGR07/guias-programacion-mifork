<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

1. En orientación a objetos, ¿qué es la herencia y su relación con "A 
es-un B"? Explica las dos implicaciones principales: (1) compatibilidad 
de tipos y (2) herencia de estado y comportamiento. Pon un ejemplo en 
Java muy sencillo… 
La herencia es un mecanismo por el cual una clase (subclase) adquiere 
automáticamente los atributos y métodos de otra clase (superclase). La relación “A 
es-un B” significa que todo objeto de la subclase puede ser tratado como un objeto 
de la superclase. 
Implicaciones principales: 
1. Compatibilidad de tipos (polimorfismo de subtipo) Si Artillero extiende 
Soldado, entonces un Artillero es un Soldado. Por tanto, una referencia de tipo 
Soldado puede apuntar a un Artillero o a un Zapador. 
2. Herencia de estado y comportamiento Las subclases heredan los atributos 
(estado) y métodos (comportamiento) de la superclase. Pueden añadir nuevos 
atributos/métodos o redefinir métodos heredados. 
Ejemplo en Java: 
java 
class Soldado { 
    private String nombre; 
 
    public Soldado(String nombre) { 
        this.nombre = nombre; 
    } 
 
    public void saludar() { 
        System.out.println("Soy " + nombre); 
    } 
} 
 
class Artillero extends Soldado { 
    private int cohetes; 
 
    public Artillero(String nombre, int cohetes) { 
        super(nombre); 
        this.cohetes = cohetes; 
    } 
 
    public int getCohetes() { 
        return cohetes; 
    } 
} 
 
class Zapador extends Soldado { 
    private int minas; 
 
    public Zapador(String nombre, int minas) { 
        super(nombre); 
        this.minas = minas; 
    } 
 
    public int getMinas() { 
        return minas; 
    } 
} 
 
public class Main { 
    public static void main(String[] args) { 
        Soldado[] escuadra = { 
            new Artillero("Luis", 5), 
            new Zapador("Ana", 3), 
            new Soldado("Pedro") 
        }; 
 
        for (Soldado s : escuadra) { 
            s.saludar(); // todos pueden saludar 
        } 
    } 
} 
 
2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan 
y en qué orden? ¿Qué significa super dentro de un constructor? Si la 
clase base no tiene visible el constructor sin parámetros, ¿debo llamar 
a super siempre? 
Cuando se crea un objeto de una subclase, se ejecutan tantos constructores como 
niveles de herencia haya, empezando por la superclase y terminando en la subclase. 
El orden es siempre: 1. Constructor de la superclase 2. Constructor de la subclase 
super(...) significa “llama explícitamente al constructor de la superclase”. 
Si la superclase no tiene un constructor sin parámetros accesible, entonces sí, 
debes llamar a super(...) explícitamente con los parámetros adecuados, porque Java 
insertaría automáticamente super() y fallaría si no existe. 
3. Respecto a los objetos de subclases en memoria, los atributos 
privados de la superclase, ¿forman parte de una instancia de la 
subclase en memoria? ¿Implica que se puedan usar desde el código de 
la subclase? 
Sí, los atributos privados de la superclase forman parte física del objeto de la 
subclase en memoria. Pero no son accesibles desde el código de la subclase, 
porque la visibilidad private lo impide. 
Ejemplo: Un Artillero tiene internamente el atributo nombre heredado de Soldado, pero 
no puede acceder a él directamente: 
java 
class Artillero extends Soldado { 
public void prueba() { 
// nombre; // ERROR: nombre es private en Soldado 
} 
} 
Aunque el atributo está en memoria, solo es accesible mediante métodos públicos o 
protegidos de la superclase. 
4. ¿Qué implica en términos de extensibilidad de código el hecho de 
que sean compatibles a nivel de tipos? Ilustra esto añadiendo un 
nuevo tipo de Soldado… 
La compatibilidad de tipos permite extender el sistema sin modificar el código 
existente que trabaja con el supertipo. Esto es un principio clave de extensibilidad: 
puedo añadir nuevas subclases sin tocar el código que opera sobre Soldado. 
Ejemplo: añadimos un nuevo tipo: 
java 
class Francotirador extends Soldado { 
public Francotirador(String nombre) { 
super(nombre); 
} 
} 
El código que recorre el array no cambia: 
java 
Soldado[] escuadra = { 
new Artillero("Luis", 5), 
new Zapador("Ana", 3), 
new Francotirador("Mario") 
}; 
for (Soldado s : escuadra) { 
s.saludar(); // funciona sin modificar nada 
} 
5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener 
una referencia del supertipo que apunte a objetos reales del subtipo? 
¿Puedo invocar con la referencia del supertipo a métodos públicos del 
subtipo? ¿En qué consiste el upcasting y el downcasting? ¿Qué es el 
instanceof? Ejemplo… 
Sí, una referencia del supertipo puede apuntar a objetos del subtipo (upcasting). No, 
con esa referencia solo puedes invocar métodos definidos en el supertipo (salvo que 
hagas downcasting). 
Upcasting: Conversión implícita de subtipo a supertipo. Siempre segura. 
Downcasting: Conversión explícita de supertipo a subtipo. Puede fallar en tiempo de 
ejecución si el objeto real no es del subtipo. 
instanceof: Permite comprobar si un objeto es instancia real de un tipo. 
Ejemplo: 
java 
for (Soldado s : escuadra) { 
s.saludar(); 
 
    if (s instanceof Artillero) { 
        Artillero a = (Artillero) s; // downcasting 
        System.out.println("Cohetes: " + a.getCohetes()); 
    } 
} 
 
6. Respecto a la ocultación de información y herencia, ¿qué significa 
acceso "protegido"? ¿Cómo se implementa en Java? Ejemplo… 
El acceso protegido (protected) permite que un atributo o método sea accesible: 
• desde la propia clase, 
• desde sus subclases, 
• desde clases del mismo paquete. 
En Java se implementa con la palabra clave protected. 
Ejemplo: permitir que Zapador use el nombre: 
java 
class Soldado { 
    protected String nombre; // ahora es accesible en subclases 
 
    public Soldado(String nombre) { 
        this.nombre = nombre; 
    } 
} 
 
class Zapador extends Soldado { 
    public Zapador(String nombre, int minas) { 
        super(nombre); 
    } 
 
    public void ponerMina() { 
        System.out.println(nombre + " ha puesto una mina"); 
    } 
} 
 
7. En los lenguajes orientados a objetos ¿hay una clase base para 
todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en 
Java? 
Depende del lenguaje. 
• En algunos lenguajes sí existe una clase raíz única (por ejemplo, Java y C#). 
• En otros lenguajes no existe una única clase base (por ejemplo, C++). 
En Java, todas las clases heredan directa o indirectamente de java.lang.Object, 
lo que garantiza un conjunto mínimo de métodos comunes (equals, hashCode, 
toString, etc.). 
8. ¿Qué es la "herencia múltiple"? ¿Existe en Java? 
La herencia múltiple permite que una clase tenga más de una superclase. 
Java no permite herencia múltiple de clases, pero sí permite herencia múltiple de 
interfaces, lo que evita problemas clásicos como la ambigüedad del diamante. 
9. Excepciones personalizadas: ejemplo 
UsuarioNoEncontradoException… 
Ejemplo de excepción no controlada (extiende RuntimeException), compuesta con un 
Usuario y con constructor que admite causa: 
java 
class Usuario { 
    private String nombre; 
    public Usuario(String nombre) { this.nombre = nombre; } 
    public String getNombre() { return nombre; } 
} 
 
class UsuarioNoEncontradoException extends RuntimeException { 
    private Usuario usuario; 
 
    public UsuarioNoEncontradoException(Usuario usuario) { 
        super("Usuario no encontrado: " + usuario.getNombre()); 
        this.usuario = usuario; 
    } 
 
    public UsuarioNoEncontradoException(Usuario usuario, Throwable 
causa) { 
super("Usuario no encontrado: " + usuario.getNombre(), 
causa); 
} 
this.usuario = usuario; 
public Usuario getUsuario() { 
return usuario; 
} 
} 

10. Herencia vs. Composición: ¿por qué no usar herencia solo para 
reutilizar código? 
Porque la herencia implica una relación conceptual fuerte (“es-un”), no solo 
reutilización. Usarla solo para compartir código: 
• acopla innecesariamente las clases, 
• limita la flexibilidad, 
• obliga a heredar comportamientos no deseados, 
• dificulta cambios futuros. 
La composición permite reutilizar código sin imponer una relación jerárquica. 

11. Herencia vs. Composición: “favorecer la composición frente a la 
herencia”, ¿por qué? 
Porque la composición: 
• es más flexible, 
• permite cambiar componentes en tiempo de ejecución, 
• evita acoplamiento fuerte, 
• respeta mejor la encapsulación, 
• facilita pruebas y mantenimiento. 
La herencia fija una estructura rígida y permanente. 

12. Herencia vs. Composición: “la herencia rompe la encapsulación”, 
¿a qué se refiere? 
La subclase depende de detalles internos de la superclase. Si la superclase cambia su 
implementación, la subclase puede romperse. Esto viola la idea de que los detalles 
internos deben estar ocultos. 
Con composición, los objetos colaboran mediante interfaces bien definidas, sin 
exponer detalles internos. 
13. Ejemplo: Estudiante y Trabajador con herencia y con composición 
Opción 1: Herencia 
java 
class Persona { 
    protected String dni; 
    protected String nombre; 
 
    public Persona(String dni, String nombre) { 
        this.dni = dni; 
        this.nombre = nombre; 
    } 
} 
 
class Estudiante extends Persona { 
    public Estudiante(String dni, String nombre) { 
        super(dni, nombre); 
    } 
} 
 
class Trabajador extends Persona { 
    public Trabajador(String dni, String nombre) { 
        super(dni, nombre); 
    } 
} 
 
Opción 2: Composición 
java 
class DatosPersonales { 
    private String dni; 
    private String nombre; 
 
    public DatosPersonales(String dni, String nombre) { 
        this.dni = dni; 
        this.nombre = nombre; 
    } 
 
    public String getDni() { return dni; } 
    public String getNombre() { return nombre; } 
} 
 
class Estudiante { 
    private DatosPersonales datos; 
 
    public Estudiante(DatosPersonales datos) { 
        this.datos = datos; 
    } 
} 
 
class Trabajador { 
    private DatosPersonales datos; 
 
    public Trabajador(DatosPersonales datos) { 
        this.datos = datos; 
    }