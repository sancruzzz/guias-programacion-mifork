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

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características de la POO son:

La **abstracción** simplifica la realidad definiendo solo los datos y funciones relevantes para el modelo. El **encapsulamiento** agrupa esos elementos en una unidad y protege el acceso a los datos internos, similar a ocultar la lógica de una estructura.

La **herencia** permite que una clase herede propiedades de otra para reutilizar código de forma jerárquica. El **polimorfismo** permite que una misma orden produzca resultados distintos según el objeto que la reciba, aportando flexibilidad al sistema.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Los cuatro lenguajes son:

Java

C++

Python

C#


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** organiza el código mediante secuencias, selecciones (`if`) e iteraciones (`for/while`). Su objetivo es que el flujo sea lineal y predecible, evitando saltos desordenados.

La **programación modular** divide el programa en partes independientes o módulos (como los archivos `.c` y `.h` en C). Cada módulo agrupa funciones relacionadas, facilitando la depuración y la reutilización de código sin mezclar toda la lógica.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Los tres elementos que definen a un objeto son:

1. **Estado**: Representado por los **atributos** (variables), define los datos o características que posee el objeto en un momento dado.
2. **Comportamiento**: Definido por los **métodos** (funciones), determina las acciones que el objeto puede realizar o cómo reacciona a estímulos.
3. **Identidad**: Es la propiedad que distingue a un objeto de otro, incluso si tienen el mismo estado; en memoria, cada instancia es única.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es el molde o plantilla (similar a una `struct` en C) que define los atributos y métodos comunes. Un **objeto** no es lo mismo que una clase; es la entidad concreta creada a partir de ese molde que ocupa espacio en memoria.

La **instancia** es el sinónimo técnico de objeto: el proceso de "instanciar" es crear un objeto específico de una clase. Aunque la mayoría de lenguajes populares (Java, C++, C#) usan clases, no todos los orientados a objetos las manejan; existen lenguajes como **JavaScript** que se basan en prototipos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

Los objetos suelen almacenarse en el **Heap** (memoria dinámica), mientras que las referencias a ellos residen en el **Stack**. Esta gestión varía: lenguajes como **C++** permiten elegir entre Stack o Heap, pero otros como **Java** obligan a usar el Heap para objetos.

La **recolección de basura** (*Garbage Collection*) es un proceso automático que libera la memoria ocupada por objetos que ya no se utilizan. Esto evita fugas de memoria sin que el programador tenga que liberar el espacio manualmente (como el `free` en C).


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función definida dentro de una clase que representa el comportamiento de un objeto. A diferencia de las funciones en C, los métodos tienen acceso directo a los datos internos (atributos) del objeto que los invoca.

La **sobrecarga de métodos** permite definir varias funciones con el mismo nombre dentro de una clase, siempre que su lista de parámetros (cantidad o tipo) sea diferente. El compilador decide qué versión ejecutar basándose en los argumentos que se le pasan al llamarla.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

class Punto {
    double x, y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3; p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es el método `public static void main(String[] args)`.

`static` indica que un miembro pertenece a la **clase** y no a una instancia específica; se puede usar sin crear objetos. No es exclusivo de `main`; se emplea en atributos o métodos compartidos por todos los objetos (ej. contadores).

Al combinar `static final`, se crean **constantes de clase**. El valor es inalterable (`final`) y existe una única copia para toda la clase (`static`), optimizando la memoria.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar se usan estos comandos en la terminal:
`javac Punto.java` (compila el código fuente)
`java Main` (ejecuta el programa)

Java es **híbrido**: se compila a un código intermedio llamado **bytecode** (archivos `.class`), que no es lenguaje máquina directo, sino instrucciones para la **Máquina Virtual de Java (JVM)**.

La **JVM** actúa como un traductor que permite ejecutar el mismo `.class` en cualquier sistema operativo, logrando la portabilidad del lenguaje.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

`new` es el operador que reserva memoria en el **Heap** para crear un objeto e invoca a su constructor. Un **constructor** es un método especial que inicializa el estado del objeto al nacer; se llama igual que la clase y no tiene tipo de retorno.

```java
class Empleado {
    String dni, nombre, apellidos;

    Empleado(String d, String n, String a) {
        dni = d; nombre = n; apellidos = a;
    }
}

```


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

`this` es una referencia que un objeto utiliza para referirse a **sí mismo**. Permite diferenciar los atributos de la clase de los parámetros locales cuando tienen el mismo nombre.

No se llama igual en todos los lenguajes: en **Python** se usa `self`, y en **PHP** se escribe `$this`.

```java
class Punto {
    double x, y;
    Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y;
    }
}

```


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Para calcular la distancia entre dos puntos, se utiliza la fórmula de la distancia euclidiana aplicada a los atributos de `this` y del punto recibido por parámetro:

```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

```

Este método accede a las coordenadas de ambos objetos para realizar el cálculo.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, el paso de parámetros es siempre **por valor**, pero lo que se copia depende del tipo:

Para el `Punto`, se copia la **referencia** (dirección de memoria). Si cambias un atributo dentro del método, el cambio **sí afecta** al objeto original fuera, porque ambos apuntan al mismo lugar en el Heap.

Para un `int`, se copia el **valor literal**. Cualquier modificación interna **no afecta** a la variable original, ya que son espacios de memoria distintos e independientes.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método `toString()` es una función heredada de la clase `Object` que devuelve una representación en **texto** de un objeto. Se invoca automáticamente cuando intentas imprimir el objeto o concatenarlo con un String.

Existe en casi todos los lenguajes: en **Python** es `__str__`, en **C#** es `ToString()` y en **JavaScript** `toString()`.

```java
@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}

```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Sí, una clase es conceptualmente similar a un `struct`, pero mucho más potente. Al `struct` de C le faltan principalmente dos cosas para ser una clase:

1. **Métodos**: Un `struct` solo almacena datos; las clases integran funciones (comportamiento) que operan sobre esos datos.
2. **Encapsulamiento**: Las clases permiten ocultar datos (usando `private`), mientras que en un `struct` todo es accesible públicamente.

Para que las variables sean "instancias", el lenguaje debe gestionar automáticamente la inicialización (constructores) y el ciclo de vida del objeto.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emular una clase, definimos un `struct` para los datos y una función externa que reciba un puntero a dicha estructura para el comportamiento:

```c
typedef struct { double x, y; } Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

```

En este caso, el puntero `Punto* p` hace el papel de **`this`**. El lenguaje no lo pasa automáticamente, por lo que debemos enviarlo explícitamente como primer argumento para saber sobre qué "instancia" operar.