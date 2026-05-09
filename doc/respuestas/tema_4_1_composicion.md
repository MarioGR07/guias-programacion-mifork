<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


TEMA 4.1 – Composición (Enunciado + 
Respuesta) 
1. En C, ejemplo de composición: una línea hecha de 
dos puntos. Incluye funciones de distancia. 
Respuesta: 
c 
#include <stdio.h> 
#include <math.h> 
typedef struct { 
double x; 
double y; 
} Punto; 
typedef struct { 
Punto p1; 
Punto p2; 
} Linea; 
double distancia(Punto a, Punto b) { 
double dx = a.x - b.x; 
double dy = a.y - b.y; 
return sqrt(dx*dx + dy*dy); 
} 
double longitudLinea(Linea l) { 
return distancia(l.p1, l.p2); 
} 
2. Transformación a Java con composición y objetos 
inmutables. 
Respuesta: 
java 
public final class Punto { 
    private final double x; 
    private final double y; 
 
    public Punto(double x, double y) { 
        this.x = x; 
        this.y = y; 
    } 
 
    public double distanciaA(Punto otro) { 
        double dx = this.x - otro.x; 
        double dy = this.y - otro.y; 
        return Math.sqrt(dx*dx + dy*dy); 
    } 
} 
 
public final class Linea { 
    private final Punto p1; 
    private final Punto p2; 
 
    public Linea(Punto p1, Punto p2) { 
        this.p1 = p1; 
        this.p2 = p2; 
    } 
 
    public double longitud() { 
        return p1.distanciaA(p2); 
    } 
} 
 
Ambas clases son inmutables: no hay setters y los atributos son final. 
3. ¿Qué significa la multiplicidad en la composición? 
¿Cuál es la del ejemplo? 
Respuesta: La multiplicidad indica cuántos objetos de un tipo están relacionados con 
otro. 
En el ejemplo: 
• Una Linea tiene exactamente 2 Puntos → multiplicidad 2. 
• Un Punto puede pertenecer a 0 o varias Líneas → multiplicidad 0..*. 
4. Composición fuerte vs débil. Consecuencias. 
Respuesta: 
• Composición fuerte: 
o El objeto contenido no puede existir sin el contenedor. 
o El ciclo de vida está ligado. 
o Es lo que solemos llamar composición. 
• Composición débil (agregación): 
o El objeto contenido puede existir independientemente. 
o No hay dependencia de ciclo de vida. 
o Se suele llamar asociación o agregación. 
5. Si una clase usa otra como parámetro o variable 
local, ¿es composición o dependencia? 
Respuesta: Eso es dependencia, no composición. La composición implica que el 
objeto forma parte del estado interno. 
6. Implementa Linea–Punto como composición fuerte y 
débil. 
Respuesta: 
Composición fuerte (Linea crea sus propios puntos) 
java 
public class Linea { 
private final Punto p1; 
private final Punto p2; 
public Linea(double x1, double y1, double x2, double y2) { 
this.p1 = new Punto(x1, y1); 
this.p2 = new Punto(x2, y2); 
} 
} 
Composición débil (Linea recibe puntos externos) 
java 
public class Linea { 
    private final Punto p1; 
    private final Punto p2; 
 
    public Linea(Punto p1, Punto p2) { 
        this.p1 = p1; 
        this.p2 = p2; 
    } 
} 
 
7. En composición fuerte, ¿cuándo destruye Java los 
objetos? ¿Por qué no se ve? 
Respuesta: Java no destruye objetos explícitamente. Los destruye el garbage 
collector cuando ya no hay referencias a ellos. Por eso no vemos un delete como en 
C++. 
8. Ejemplo de composición débil: Departamento con 
Profesores y un Director. 
Respuesta: 
java 
public class Profesor { 
    private final String nombre; 
 
    public Profesor(String nombre) { 
        this.nombre = nombre; 
    } 
 
    public String getNombre() { return nombre; } 
} 
 
public class Departamento { 
    private Profesor[] profesores = new Profesor[50]; 
    private int numProfesores = 0; 
    private Profesor director; 
 
    public Departamento(Profesor directorInicial) { 
        if (directorInicial == null) 
            throw new IllegalArgumentException("Debe haber 
director"); 
        this.director = directorInicial; 
        profesores[numProfesores++] = directorInicial; 
    } 
 
    public void addProfesor(Profesor p) { 
        if (numProfesores >= 50) 
            throw new IllegalStateException("Departamento lleno"); 
        profesores[numProfesores++] = p; 
    } 
 
    public void removeProfesor(int pos) { 
        if (pos < 0 || pos >= numProfesores) 
            throw new IllegalArgumentException("Posición inválida"); 
 
        if (profesores[pos] == director) 
            throw new IllegalStateException("No se puede eliminar al 
director"); 
 
        for (int i = pos; i < numProfesores - 1; i++) 
            profesores[i] = profesores[i+1]; 
 
        numProfesores--; 
    } 
 
    public int getNumProfesores() { return numProfesores; } 
 
    public Profesor getProfesor(int pos) { 
        if (pos < 0 || pos >= numProfesores) 
            throw new IllegalArgumentException("Posición inválida"); 
        return profesores[pos]; 
    } 
 
    public void cambiarDirector(Profesor nuevo) { 
        boolean encontrado = false; 
        for (int i = 0; i < numProfesores; i++) 
            if (profesores[i] == nuevo) 
                encontrado = true; 
 
        if (!encontrado) 
            throw new IllegalArgumentException("El director debe ser 
profesor del departamento"); 
 
        director = nuevo; 
    } 
} 
 
9. Versión usando List. ¿Qué parte se simplifica? 
¿Problema de devolver la lista interna? 
Respuesta: 
java 
import java.util.*; 
 
public class Departamento { 
    private List<Profesor> profesores = new ArrayList<>(); 
    private Profesor director; 
 
    public Departamento(Profesor directorInicial) { 
        director = directorInicial; 
        profesores.add(directorInicial); 
    } 
 
    public void addProfesor(Profesor p) { 
        profesores.add(p); 
    } 
 
    public void removeProfesor(int pos) { 
        Profesor p = profesores.get(pos); 
        if (p == director) 
            throw new IllegalStateException("No se puede eliminar al 
director"); 
        profesores.remove(pos); 
    } 
 
    public Profesor getProfesor(int pos) { 
        return profesores.get(pos); 
    } 
} 
 
¿Qué nos ahorramos? 
• Gestión manual del tamaño. 
• Desplazar elementos al eliminar. 
• Comprobar límites manualmente. 
¿Problema de devolver la lista interna? 
El usuario podría modificarla desde fuera, rompiendo invariantes. 
Solución: 
Devolver una copia o una vista inmodificable: 
java 
public List<Profesor> getProfesores() { 
    return Collections.unmodifiableList(profesores); 
} 
 
10. Ejemplo de composición recursiva: Persona con 
madre. 
Respuesta: 
java 
public final class Persona { 
    private final String nombre; 
    private final Persona madre; 
 
    public Persona(String nombre, Persona madre) { 
        this.nombre = nombre; 
        this.madre = madre; 
    } 
 
    public String getNombre() { return nombre; } 
    public Persona getMadre() { return madre; } 
} 
 
public class Main { 
    public static void main(String[] args) { 
        Persona abuela = new Persona("Ana", null); 
        Persona madre = new Persona("Laura", abuela); 
        Persona hijo = new Persona("Carlos", madre); 
 
        System.out.println(hijo.getMadre().getNombre()); // Laura 
        System.out.println(hijo.getMadre().getMadre().getNombre()); 
// Ana 
    } 
} 
 
Otros ejemplos clásicos de composición recursiva: 
• Árboles (nodos con hijos). 
• Carpetas y subcarpetas. 
• Expresiones matemáticas (un nodo contiene otros nodos). 
• Excepciones con causa. 

11. ¿Qué son las composiciones bidireccionales? 
¿Cómo implementarlas en Profesor–Departamento? 
Respuesta: Una composición bidireccional es aquella donde cada objeto conoce al 
otro. 
En Profesor–Departamento: 
• El Departamento tiene una lista de Profesores. 
• Cada Profesor tiene una referencia a su Departamento. 
Ejemplo: 
java 
public class Profesor { 
    private Departamento dept; 
 
    void setDepartamento(Departamento d) { 
        this.dept = d; 
    } 
} 
public class Departamento { 
public void addProfesor(Profesor p) { 
profesores.add(p); 
p.setDepartamento(this); 
} 
} 
Hay que tener cuidado para mantener la coherencia en ambos lados