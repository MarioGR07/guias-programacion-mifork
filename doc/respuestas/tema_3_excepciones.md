<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

TEMA 3 – Excepciones (Enunciado + 
Respuesta) 
1. En C, sin excepciones, queremos controlar el error 
de una raíz con número negativo. ¿Cómo indicamos el 
error? Da dos opciones con ejemplo. 
Respuesta: 
Opción 1: devolver un valor especial (sentinela) 
c 
#include <math.h> 
#include <stdio.h> 
 
double raiz(double x) { 
    if (x < 0) return -1.0; // valor especial indicando error 
    return sqrt(x); 
} 
 
int main() { 
    double r = raiz(-9); 
    if (r == -1.0) printf("Error: número negativo\n"); 
} 
 
Opción 2: usar un parámetro de salida para indicar error 
c 
#include <math.h> 
#include <stdio.h> 
 
double raiz(double x, int *error) { 
    if (x < 0) { 
        *error = 1; 
        return 0; 
    } 
    *error = 0; 
    return sqrt(x); 
} 
 
int main() { 
    int err; 
    double r = raiz(-9, &err); 
    if (err) printf("Error: número negativo\n"); 
} 
 
2. ¿Qué es una excepción? ¿Para qué se usa? 
Respuesta: Una excepción es un mecanismo que permite interrumpir el flujo normal 
del programa cuando ocurre un error. Se usa para: 
• Señalar errores sin mezclar lógica normal con lógica de error. 
• Delegar el manejo del error en quien llame a la función. 
3. Reescribe el ejemplo de raíz en Java con clase 
Calculadora y control desde main. 
Respuesta: 
java 
public class Calculadora { 
    public static double raiz(double x) { 
        if (x < 0) throw new IllegalArgumentException("Número 
negativo"); 
        return Math.sqrt(x); 
    } 
 
    public static void main(String[] args) { 
        try { 
            double r = Calculadora.raiz(-9); 
            System.out.println(r); 
        } catch (IllegalArgumentException e) { 
            System.out.println("Error: " + e.getMessage()); 
        } 
    } 
} 
4. ¿Qué es lanzar, capturar y propagar una excepción? 
¿Qué pasa en la pila? 
Respuesta: 
• Lanzar: crear y emitir una excepción con throw. 
• Capturar: manejarla con try-catch. 
• Propagar: si no se captura, sube por la pila de llamadas. 
• Las funciones por las que pasa no se reanudan, se abortan hasta encontrar un 
catch. 
Ejemplo: raiz() lanza → main() captura → el resto de funciones intermedias se 
abortan. 
5. ¿Qué ventajas tiene la propagación natural frente a 
C? 
Respuesta: 
• No hay que comprobar códigos de error manualmente. 
• El error sube automáticamente hasta quien pueda manejarlo. 
• El código normal queda más limpio. 
6. ¿Las excepciones suelen ser objetos? ¿Ventajas? 
¿Podemos crear excepciones personalizadas? 
Respuesta: Sí, son objetos. Ventajas: 
• Encapsulan mensaje, causa, tipo, etc. 
• Permiten jerarquías de errores. Sí, podemos crear excepciones propias 
extendiendo Exception o RuntimeException. 
7. ¿Qué información esencial lleva una excepción útil 
para el manejador? 
Respuesta: 
• Mensaje descriptivo. 
• Tipo de excepción. 
• Trazado de pila (stack trace). 
• Causa original (otra excepción interna). 
8. ¿Se pueden tener varios catch? ¿Cuántos se 
ejecutan? 
Respuesta: Sí, se pueden tener varios. Solo uno se ejecuta: el primero que coincida 
con el tipo de excepción. 
9. ¿Cómo garantizar ejecución final (cierre de 
recursos)? Ejemplo con finally. 
Respuesta: 
Con catch 
java 
try { 
abrirFichero(); 
} catch (IOException e) { 
System.out.println("Error"); 
} finally { 
cerrarFichero(); 
} 
Sin catch 
java 
try { 
abrirFichero(); 
} finally { 
cerrarFichero(); 
} 
10. ¿finally puede ir sin catch? ¿Se ejecuta siempre? 
¿Incluso con return? 
Respuesta: 
• Sí, puede ir sin catch. 
• Sí, se ejecuta siempre, ocurra o no excepción. 
• Incluso si hay un return dentro del try. 
11. Excepciones controladas y no controladas. Papel 
de RuntimeException. Ejemplos. 
Respuesta: 
• Controladas (checked): deben declararse o capturarse. 
• No controladas (unchecked): heredan de RuntimeException. 
Ejemplos controladas: 
• IOException 
• SQLException 
• FileNotFoundException 
Ejemplos no controladas: 
• IllegalArgumentException 
• NullPointerException 
• ArithmeticException 
Cuándo usar controladas: 
1. Errores de E/S. 
2. Recursos externos. 
3. Operaciones que pueden fallar por causas externas. 
Cuándo usar no controladas: 
1. Argumentos inválidos. 
2. Errores de programación. 
3. Estados imposibles. 
12. ¿Qué es throws? ¿Por qué es alternativa a 
capturar? 
Respuesta: throws indica que un método no maneja una excepción controlada y la 
deja propagar. Es alternativa a capturar porque delega el manejo al llamador. 
13. Ejemplo de método con throws que abre un fichero 
y usa finally. 
Respuesta: 
java 
public void leerArchivo(String nombre) throws IOException { 
FileReader fr = null; 
try { 
fr = new FileReader(nombre); 
// leer... 
} finally { 
if (fr != null) fr.close(); 
} 
} 
14. ¿Podemos poner excepciones no controladas en 
throws? ¿Debe el llamador capturarlas? 
Respuesta: Sí, se puede, pero no es obligatorio capturarlas. Tiene sentido si 
queremos documentar que el método puede lanzar una excepción no controlada. 
15. ¿Cuándo usar controladas y cuándo no 
controladas? ¿Todos los lenguajes tienen ambas? 
Respuesta: 
• Controladas: cuando el error es esperable y recuperable (E/S, red). 
• No controladas: errores de programación o argumentos inválidos. No todos los 
lenguajes tienen checked exceptions (Python, C#, JavaScript no). La opción 
habitual en esos lenguajes es no controladas. 
16. ¿Tiene sentido lanzar excepciones dentro del 
catch? ¿Se puede relanzar? 
Respuesta: Sí. 
Lanzar otra excepción: 
java 
catch (IOException e) { 
throw new RuntimeException("Error leyendo", e); 
} 
Relanzar la misma: 
java 
catch (IOException e) { 
System.out.println("Log del error"); 
throw e; 
} 
Se usa cuando queremos registrar o añadir información antes de propagar. 
17. ¿Qué es que una excepción sea la causa de otra? 
Ejemplo. ¿Se ve al imprimir? 
Respuesta: Una excepción puede contener otra como causa, preservando el error 
original. 
Ejemplo: 
java 
try { 
leerArchivo(); 
} catch (IOException e) { 
throw new MiExcepcion("Fallo de alto nivel", e); 
} 
Sí, al imprimir la excepción aparece el stack trace completo, incluyendo la causa.