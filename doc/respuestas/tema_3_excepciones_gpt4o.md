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

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al no existir excepciones, el control de errores se basa en la comunicación mediante valores de retorno o indicadores externos.

### Opción 1: Valor de Retorno Especial

Se reserva un valor específico del rango de salida (como un número negativo o `NaN`) para indicar que los parámetros de entrada eran inválidos. El llamador debe verificar este valor antes de usar el resultado.

```c
float raiz(float n) {
    if (n < 0) return -1.0; // Valor centinela
    return sqrt(n);
}

```

### Opción 2: Puntero de Estado o Variable Global

La función devuelve un código de error (entero) y entrega el resultado matemático a través de un puntero pasado por referencia. Esto separa el éxito de la operación del dato obtenido.

```c
int raiz(float n, float *res) {
    if (n < 0) return 0; // Error: devuelve falso
    *res = sqrt(n);
    return 1; // Éxito
}

```


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un objeto que representa un evento anormal durante la ejecución, interrumpiendo el flujo secuencial de instrucciones. A diferencia de C, donde se usan valores de retorno, Java encapsula el error en una entidad con datos propios.

Al **implementar** una función, el objetivo es notificar de forma automática que una condición previa no se cumple (como un divisor cero). Al **llamarla**, el objetivo es capturar dicho evento para aplicar una lógica de recuperación, evitando que el programa finalice abruptamente.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, se utiliza la palabra clave `throw` para lanzar un objeto de excepción cuando se detecta un valor negativo. El método debe declarar esta posibilidad en su firma mediante `throws`, delegando la responsabilidad del error al llamador.

Desde el método `main`, se rodea la llamada con un bloque `try-catch`. Si la función lanza la excepción, el flujo salta directamente al bloque `catch`, donde se gestiona el mensaje informativo sin interrumpir la ejecución del resto del programa.

```java
class Calculadora {
    public double raiz(double n) throws Exception {
        if (n < 0) throw new Exception("Número negativo");
        return Math.sqrt(n);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            System.out.println(calc.raiz(-5));
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

```


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**Lanzar** es activar la señal de error mediante `throw` (ej. `throw new Exception()`). **Capturar** es atrapar esa señal con `catch` para gestionar el fallo. **Propagar** es dejar que el error viaje hacia atrás en la pila de llamadas si la función actual no lo captura.

En la pila, las funciones se interrumpen inmediatamente al recibir la excepción. Las funciones que no la controlan **no se reanudan** jamás; se cierran abruptamente una tras otra hasta encontrar un `catch` o detener el programa. En el ejemplo, si `raiz` lanza el error, cualquier código posterior en `raiz` o en el `try` de `main` queda anulado.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

En C, el error debe comprobarse manualmente con `if` tras cada llamada, ensuciando la lógica principal. La **propagación natural** en Java permite que el error "suba" solo hasta donde se puede gestionar, permitiendo funciones intermedias más limpias y centradas solo en su tarea.

Esto garantiza robustez: si un programador olvida un `if` en C, el programa sigue con datos corruptos; en Java, la excepción **obliga** a una respuesta o detiene el programa, evitando estados inconsistentes de forma segura y automática.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en Java las excepciones **son objetos** que instancian clases de una jerarquía (bajo `Throwable`). Esto permite que el error no sea solo un número (como en C), sino una entidad con estado y comportamiento propio.

En términos de **encapsulación**, el objeto agrupa el tipo de error, el mensaje y la traza de la pila en un solo paquete. Esto evita el uso de variables globales y permite pasar información detallada del fallo sin exponer la lógica interna de la función.

Es posible crear **excepciones personalizadas** simplemente heredando de una clase existente (como `Exception`). Esto permite definir errores específicos para tu dominio, facilitando un control más preciso y semántico en los bloques `catch`.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Un objeto excepción contiene principalmente tres elementos: el **tipo de error** (nombre de la clase), un **mensaje descriptivo** opcional y la **traza de la pila** (*stack trace*).

Esta información permite identificar la causa exacta y el punto preciso del código donde se originó el fallo, recorriendo todas las funciones activas en ese instante. En C, obtener esta trazabilidad requeriría una gestión manual compleja, mientras que en Java el objeto lo proporciona de forma automática al manejador.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Es posible encadenar múltiples bloques `catch` tras un único `try` para gestionar distintos tipos de excepciones de forma específica. Cada bloque debe declarar un tipo de excepción diferente, permitiendo una respuesta personalizada según el error ocurrido (ej. uno para errores matemáticos y otro para errores de entrada/salida).

Solo se ejecuta el **primer bloque catch** que coincida con el tipo de excepción lanzada o con una de sus clases padre. Una vez que un bloque captura la excepción, el resto de los bloques `catch` asociados a ese `try` se ignoran por completo, continuando la ejecución después de la estructura.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de código crítico, se utiliza el bloque **`finally`**. Este se ejecuta siempre, independientemente de si se lanzó una excepción o si esta fue capturada, lo que asegura el cierre de recursos o la liberación de memoria antes de que el control pase a otra parte del programa.

Si se usa con `catch`, el bloque `finally` se ejecuta después del manejador; si se usa sin él, el código de limpieza se ejecuta justo antes de que la excepción continúe su propagación hacia la función llamadora.

```java
// Con catch: captura y limpia
try { 
    abrirArchivo(); 
} catch (Exception e) { 
    System.out.println("Error"); 
} finally { 
    cerrarArchivo(); 
}

// Sin catch: limpia y propaga el error
try { 
    abrirArchivo(); 
} finally { 
    cerrarArchivo(); 
}

```


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, el bloque `finally` puede ir acompañado únicamente de `try` sin necesidad de un `catch`. Se ejecuta siempre, independientemente de si se lanza una excepción o si el código del `try` finaliza correctamente.

Incluso ante una sentencia `return` dentro del bloque `try`, Java garantiza la ejecución del `finally` antes de que el control regrese a la función llamadora. Esto asegura que la liberación de recursos o cierres de archivos se realicen de forma determinista antes de abandonar el método.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones **controladas** (*checked*) son aquellas que el compilador obliga a capturar o declarar (ej. `IOException`, `FileNotFoundException`). Se usan para fallos externos previsibles donde el programa puede recuperarse.

Las **no controladas** (*unchecked*) heredan de `RuntimeException` (ej. `NullPointerException`, `IndexOutOfBoundsException`). No requieren gestión obligatoria porque suelen indicar errores de lógica del programador que deben corregirse en el código.

**Preferir Controladas (Checked) cuando:**

* El error es externo y probable (red, archivos).
* El usuario puede subsanar el error (reintentar ruta).
* Se quiere forzar al llamador a gestionar el fallo.

**Preferir No Controladas (Unchecked) cuando:**

* Es un error de programación (puntero nulo).
* El error es irrecuperable (fallo crítico de memoria).
* Se desea evitar "ensuciar" el código con `try-catch` constantes.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La cláusula **`throws`** se añade a la firma de un método para declarar que este puede lanzar una o varias excepciones específicas. Su función es advertir al programador que llame a dicho método sobre los posibles errores que debe gestionar, trasladando la responsabilidad de la captura al nivel superior de la pila de llamadas.

Es la alternativa a capturar una excepción controlada porque permite que el método actual **no gestione el error** internamente con un `try-catch`. Al usar `throws`, el método simplemente deja pasar la excepción hacia quien lo invocó, evitando código de tratamiento de errores innecesario en funciones intermedias que no saben cómo resolver el fallo.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

Al usar `throws` en la firma, el método delega la responsabilidad de capturar `FileNotFoundException` al llamador. El bloque `finally` garantiza que, independientemente de si el archivo existe o se lanza la excepción, los recursos abiertos se cierren correctamente antes de que el error se propague por la pila.

```java
public void abrirArchivo(String ruta) throws FileNotFoundException {
    FileReader lector = null;
    try {
        lector = new FileReader(ruta);
        // Operaciones con el archivo
    } finally {
        if (lector != null) {
            System.out.println("Cerrando recurso...");
            // Lógica de cierre (omitida para brevedad)
        }
    }
}

```


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Es posible incluir excepciones no controladas como `RuntimeException` en la cláusula `throws`, aunque el compilador no lo exige. El método llamador **no está obligado** a usar `try-catch` en estos casos, a diferencia de lo que ocurre con las excepciones controladas.

El sentido de declararlas es puramente **documentativo**. Sirve para advertir a otros programadores de que el método podría fallar por causas específicas (como un argumento inválido), permitiéndoles decidir si prefieren capturarlas para evitar que el programa se detenga o dejar que se propaguen.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomiendan las **controladas** para fallos externos recuperables (red, archivos) donde se desea obligar al llamador a gestionar el error. Las **no controladas** se usan para errores de lógica (argumentos inválidos, punteros nulos) que el programador debería haber evitado mediante una codificación correcta.

No todos los lenguajes distinguen ambos tipos; Java es de los pocos que implementa excepciones controladas de forma estricta. En lenguajes como C++, C#, Python o JavaScript, **solo existen las no controladas**, siendo este el modelo más habitual en la programación moderna para evitar la verbosidad del código.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar una excepción dentro de un `catch` es una práctica común para **envolver** un error de bajo nivel en uno más significativo para la lógica del negocio. También es posible **relanzar** la misma excepción capturada si se desea realizar una acción intermedia (como registrar un *log*) sin detener la propagación del error original.

Relanzar tiene sentido cuando el método actual no puede solucionar el problema, pero necesita asegurar que se ejecute una limpieza o una notificación antes de que el error llegue al llamador.

```java
// Caso 1: Envolver en una nueva excepción
catch (SQLException e) {
    throw new Exception("Error al acceder a la base de datos");
}

// Caso 2: Relanzar la misma excepción
catch (IOException e) {
    log.error("Fallo de lectura");
    throw e; 
}

```


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Una excepción es la **"causa"** de otra cuando se envuelve un error de bajo nivel (como un fallo de red) dentro de una excepción de mayor nivel semántico (como un error de negocio). Esto se logra pasando la excepción original al constructor de la nueva, manteniendo la trazabilidad completa mediante el encadenamiento de excepciones.

```java
try {
    throw new SQLException("Error de DB");
} catch (SQLException e) {
    throw new MiExcepcionPersonalizada("Error al cargar usuario", e);
}

```

Al mostrarse por pantalla, la causa **sí se ve**. Java imprime primero la excepción principal y, a continuación, añade una sección precedida por **"Caused by:"**, detallando el error original y su propia traza de la pila para facilitar la depuración.