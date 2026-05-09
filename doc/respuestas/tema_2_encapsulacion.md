<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

TEMA 2 – Encapsulación (Enunciado + 
Respuesta) 
1. En POO, ¿Qué buscan la encapsulación y la 
ocultación de información? Enumera brevemente 
algunas ventajas. 
Respuesta: Buscan proteger el estado interno de los objetos y controlar cómo se 
accede y modifica. Ventajas: 
• Evita usos incorrectos del objeto. 
• Permite cambiar la implementación sin afectar al exterior. 
• Mejora la mantenibilidad. 
• Reduce errores y efectos colaterales. 
2. ¿Qué se entiende por la interfaz pública de un objeto 
o clase? ¿Cómo se relaciona con la ocultación? 
Respuesta: La interfaz pública es el conjunto de métodos accesibles desde fuera. La 
ocultación de información consiste en esconder los detalles internos, exponiendo 
solo esa interfaz pública. 
3. ¿Por qué hay que diseñar con cuidado la interfaz 
pública? ¿Es fácil cambiarla? 
Respuesta: Porque cualquier cambio afecta a todo el código que usa la clase. 
Cambiarla no es fácil, ya que rompe compatibilidad y obliga a modificar otros 
módulos. 
4. ¿Qué son las invariantes de clase y por qué la 
ocultación ayuda? 
Respuesta: Son condiciones que siempre deben cumplirse para que un objeto sea 
válido. La ocultación ayuda porque evita que código externo modifique atributos de 
forma incorrecta. 
5. Clase Punto en Java con ocultación de información. 
¿Cuál es su interfaz pública? ¿Qué significan public y 
private? 
Respuesta: 
java 
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
 
• Interfaz pública: constructor y método calcularDistanciaAOrigen(). 
• public: accesible desde cualquier parte. 
• private: solo accesible dentro de la clase. 
6. En Java, ¿a quiénes se pueden aplicar public o 
private? 
Respuesta: A clases, métodos, atributos y constructores. 
7. ¿Existen más tipos de visibilidad? ¿Qué ocurre en 
Java? ¿Y en otros lenguajes? 
Respuesta: Sí. En Java existen: 
• public 
• private 
• protected 
• default (sin modificador) 
Otros lenguajes pueden tener más o menos (por ejemplo, C++ tiene public, private, 
protected pero no default). 
8. Los miembros privados están ocultos para (a) otras 
clases o (b) otras instancias? 
Respuesta: Están ocultos para otras clases, pero no para otras instancias de la 
misma clase. 
Ejemplo: 
java 
public double calcularDistanciaAPunto(Punto otro) { 
double dx = this.x - otro.x; // permitido: otra instancia accede 
a x privado 
double dy = this.y - otro.y; 
return Math.sqrt(dx*dx + dy*dy); 
} 
9. ¿Qué son los métodos getter y setter? 
Respuesta: Métodos que permiten leer (getter) o modificar (setter) atributos privados 
de forma controlada. 
10. Cuando decimos que mejora la “seguridad”, 
¿significa que evita hackeos? 
Respuesta: No. Se refiere a seguridad lógica, es decir, evitar usos incorrectos del 
objeto, no a seguridad informática. 
11. Diferencia entre miembro de instancia y miembro 
de clase. ¿Los de clase pueden ocultarse? 
Respuesta: 
• Miembro de instancia: pertenece a cada objeto. 
• Miembro de clase (static): pertenece a la clase, no a los objetos. Sí, también 
pueden ser privados. 
12. ¿Tiene sentido que los constructores sean 
privados? 
Respuesta: Sí, cuando queremos controlar la creación de objetos, por ejemplo con 
métodos factoría o patrón Singleton. 
13. ¿Cómo se indican los miembros de clase en Java? 
Ejemplo con máximos x e y. 
Respuesta: Con static. 
java 
public class Punto { 
    private double x, y; 
    private static double maxX = Double.MIN_VALUE; 
    private static double maxY = Double.MIN_VALUE; 
 
    public Punto(double x, double y) { 
        this.x = x; 
        this.y = y; 
        if (x > maxX) maxX = x; 
        if (y > maxY) maxY = y; 
    } 
 
    public static double getMaxX() { return maxX; } 
    public static double getMaxY() { return maxY; } 
} 
 
14. Método factoría que redondee coordenadas. ¿Has 
usado static? 
Respuesta: 
java 
public static Punto crearRedondeado(double x, double y) { 
    return new Punto(Math.round(x), Math.round(y)); 
} 
 
Sí, es static. 
15. Cambia Punto para usar un array interno sin 
modificar la interfaz pública. 
Respuesta: 
java 
public class Punto { 
    private double[] coords = new double[2]; 
 
    public Punto(double x, double y) { 
        coords[0] = x; 
        coords[1] = y; 
    } 
 
    public double calcularDistanciaAOrigen() { 
        return Math.sqrt(coords[0]*coords[0] + coords[1]*coords[1]); 
    } 
} 
 
16. Si un atributo tiene getter y setter públicos, ¿no es 
mejor declararlo público? 
Respuesta: No. La convención es atributos privados siempre. Los getters/setters 
permiten mantener invariantes de clase y controlar cambios. 
17. ¿Qué es una clase inmutable? ¿Qué es un método 
modificador? ¿Ventajas? 
Respuesta: 
• Clase inmutable: su estado no cambia tras crearse. 
• Método modificador: cambia el estado del objeto. 
• No siempre es un setter. Ventajas: 
• Más segura. 
• Más fácil de razonar. 
• Útil en programación concurrente. 
18. ¿Es recomendable incluir setters siempre? 
Respuesta: No. Solo cuando realmente sea necesario modificar el atributo. 
19. ¿String en Java es mutable o inmutable? ¿Qué pasa 
al concatenar? 
Respuesta: 
• String es inmutable. 
• Al concatenar se crea un nuevo objeto. 
• Para muchas concatenaciones se usa StringBuilder. 
20. ¿Cómo se comparan objetos? ¿Qué es equals? 
¿Cómo comparar cadenas? 
Respuesta: 
• Por defecto, los objetos se comparan por identidad (==). 
• equals compara contenido, pero por defecto hereda de Object y compara 
identidad. 
• Las cadenas se comparan con equals, no con ==. 
21. ¿Qué son las clases wrapper? ¿Ventajas? 
Respuesta: Clases que envuelven tipos primitivos (Integer, Double…). El proceso 
puede ser automático (autoboxing). Ventajas: 
• Permiten tratarlos como objetos. 
• Necesarios para colecciones genéricas. No todos los lenguajes tienen 
primitivos (Python no). 
22. ¿Qué es un tipo enumerado? ¿En Java es una clase? 
¿Ventajas? 
Respuesta: Un tipo con un conjunto fijo de valores posibles. En Java, un enum es una 
clase especial. Ventajas: 
• Encapsulación. 
• Seguridad de tipos. 
• Métodos y atributos propios. 
23. Crea un enum Mes con días y ordinal. 
Respuesta: 
java 
public enum Mes { 
ENERO(31,1), FEBRERO(28,2), MARZO(31,3), ABRIL(30,4), 
MAYO(31,5), JUNIO(30,6), JULIO(31,7), AGOSTO(31,8), 
SEPTIEMBRE(30,9), OCTUBRE(31,10), NOVIEMBRE(30,11), 
DICIEMBRE(31,12); 
private int dias; 
private int numero; 
Mes(int dias, int numero) { 
this.dias = dias; 
this.numero = numero; 
    } 
 
    public int getDias() { return dias; } 
    public int getNumero() { return numero; } 
} 
 
24. Añade métodos para estaciones según hemisferio. 
Respuesta: 
java 
public boolean esDePrimavera(boolean norte) { 
    return norte ? (this == MARZO || this == ABRIL || this == MAYO) 
                 : (this == SEPTIEMBRE || this == OCTUBRE || this == 
NOVIEMBRE); 
} 
 
public boolean esDeVerano(boolean norte) { 
    return norte ? (this == JUNIO || this == JULIO || this == 
AGOSTO) 
                 : (this == DICIEMBRE || this == ENERO || this == 
FEBRERO); 
} 
 
public boolean esDeOtono(boolean norte) { 
    return norte ? (this == SEPTIEMBRE || this == OCTUBRE || this == 
NOVIEMBRE) 
                 : (this == MARZO || this == ABRIL || this == MAYO); 
} 
 
public boolean esDeInvierno(boolean norte) { 
    return norte ? (this == DICIEMBRE || this == ENERO || this == 
FEBRERO) 
                 : (this == JUNIO || this == JULIO || this == 
AGOSTO); 
} 