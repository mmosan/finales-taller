# Q&A Preguntas de Final

## C++: Standard Template Library y Conceptos

> ¿Qué son las librerías STL? ¿Qué ventajas ofrece? Dé un ejemplo de su uso.

STL es el acrónimo de *Standard Template Library*, una librería para el desarrrollo de programas en C++. Esta librería la componen **algoritmos**, **iteradores**, **contenedores** y **funciones**. Todas están relacionadas entre sí y dedicadas a facilitar la implementación de diversas estructuras de datos frecuentemente utilizadas en la programación.

```cpp
std::list<int> numeros = {0, -1, 5, 2, 9};
std::sort(numeros.begin(), numeros.end());
numeros.push_back(13);
std::list::iterator it;
for (it = numeros.begin(); it != numeros.end(); ++it) {
    std::cout << *it << std::endl;
}
```

---

> ¿Qué es un iterador en STL? Ejemplificar

STL es la Standard Template Library de C++. Un iterador es un objeto usado para recoorer secuencialmente los elementos de tipos contenedores, tales como un `std::vector`, una `std::list` o un `std::map`. Sirven para recorrer estas estructuras y permiten la modificación del elemento contenido, modificando al espacio de memoria apuntado.

En STL, los iteradores son usados por varios algoritmos, como por ejemplo, `std::sort`, `std::find` o los métodos `begin()` y `end()` de los contenedores, que devuelven un iterador.

```cpp
std::vector<int> numeros = {1, 2, 3};

// it es del tipo std::vector<int>::iterator.
for (auto it = numeros.begin(); it != numeros.end(); it++) {
    std::cout << *it << std::endl;
}
```

---

> ¿Qué significa que una función sea **blocante**? ¿Cómo subsanaría esa limitación en términos de **mantener el programa 'vivo'**?

Una función es bloqueante cuando detiene la ejecución del hilo hasta que termine su tarea. Por ejemplo, la función pop de la cola provista por la cátedra es una función bloqueante, ya que espera a poder sacar un elemento de la cola, y si esta está vacía, se queda esperando a que haya algún elemento para poder quitar.

Para subsanar esta limitación, se pueden lanzar múltiples hilos (multithreading) o procesos (fork).

---

> ¿Qué significa la palabra **virtual** antepuesta a un método de una clase? ¿Qué cambios genera a nivel **compilación** y al momento de **ejecución**?

La palabra `virtual` antepuesta a un método de clase significa que este método puede ser redefinido o sobreescrito en clases derivadas; es fundamental para el uso del polimorfismo ya que permite que distintos objetos sean tratados como un tipo común (la case base).

En compilación, el compilador genera una tabla virtual para cada clase que tiene punteros a las funciones virtuales de esa clase y la clase base. En tiempo de ejecución, se usa esta tabla para determinar cuál es la implementación correcta que se va a usar. El enlace de la función se determina en tiempo de ejecución (**late binding**). Es gracias a esto que se puede usar polimorfismo.

---

> ¿Qué significado tiene la palabra reservada `static` cuando es antepuesta a una variable local a una función? Ejemplifique.

La palabra `static` en dicho contexto connota que la variable no será una variable temporal de la función sino que perdurará en la ejecución de la misma. Esta variable será alojada en el segmento de datos (data segment) y se inicializará una sola vez.

---

> Explicar el efecto de anteponer la palabra `static` a un método de una clase. Ejemplifique con un código simple.

Declarar un método estático significa que el método pertenece a la clase en sí y no a una instancia, por lo que se puede acceder al método sin necesidad de crear un objeto de la clase.

```cpp
class Printer {
 public:
    static void printMessage(const std::string& msg) {
        std::cout << msg << std::endl;
    }
};

int main() {
    Printer::printMessage("Imprimí esto");
    return 0;
}
```

---

> ¿Qué es un parámetro **constante**? **Ejemplifique**.

Es un parámetro al cual no se le puede modificar su estado, solo se puede llamar a sus métodos constantes.

```cpp
int suma(const int a, const int b) {
    return a + b;
}
```

---

### Constructores

> Explique qué es y para qué sirve un **constructor de copia** en C++.
>
> - Indique cómo se comporta el sistema si éste **no es definido por el desarrollador**
> - Explique al menos una estrategia para **evitar que una clase particular sea copiable**
> - Indique qué **diferencia** existe entre un constructor de **copia** y uno **move**.

Un constructor por copia es aquel que copia miembro por miembro, manteniendo al original sin cambios y generando un segundo elemento idéntico al primero. Sirve para realizar copias, pasar por valor y para la inicialización de objetos, aunque es más ineficiente que el constructor por movimiento.

Si no es definido por el desarrrollador, el compilador genera uno por defecto, aunque puede no tener el comportamiento deseado.

Para evitar que una clase particular sea copiable hay dos maneras: declarar al constructor por copia como privado, o bien usando la keyword

```cpp
class Objeto1 {
 private:
    Objeto1(const Objeto1&);
};

class Objeto2 {
 public:
    Objeto2(const Objeto2&) = delete;
    Objeto2& operator=(const Objeto2&) = delete;
};
```

A diferencia del constructor por movimiento, el constructor por copia es más ineficiente ya que ocupa más memoria copiando miembro a miembro en lugar de utilizar el mismo objeto original cambiando su propietario; si se desea dejar el objeto original intacto, el constructor por copia es más conveniente.

---

> ¿Qué diferencia existe entre un **constructor por copia** y uno por **movimiento**? **Ejemplifique**.

En un constructor por copia, se crea una copia exacta del objeto, resultando en 2 objetos idénticos en memoria. El objeto original no se modifica.
En un constructor por movimiento, el objeto original es transferido hacia el objeto construido, transfiriéndose su propiedad. En memoria, queda el mismo objeto solo que con distinto propietario.

```cpp
void printeameEsto(std::string str) {
    std::cout << str << std::endl;
}

std::string msg = "Hola";

// Acá se invoca por copia
printeameEsto(msg);

// Acá se invoca por movimiento
printeameEsto(std::move(msg));
```

Si en el ejemplo anterior se invocaba por movimiento antes que por copia, en la invocación por copia se estaría pasando un objeto ya movido, por lo que no funcionaría.

---

> Explique qué es y para qué sirve una **variable de clase** (o atributo estático) en C++. Mediante un ejemplo de uso, indique cómo se define dicha variable, su inicialización y el acceso a su valor para realizar una impresión simple dentro de un **main**.

Las variables de clase son los atributos estáticos, que pertenecen a la clase en sí y no a una instancia de la clase misma. Sirven para guardar datos compartidos, asociar variables globales y almacenar constantes.

```cpp
class Printer {
    static std::string msgPorDefecto = "Hola";
};

int main() {
    std::cout << Printer::msgPorDefecto << std::endl;
    return 0;
}
```

---

> ¿Qué significa la palabra `const` cuando sucede a una declaración de un método de una clase? Ejemplifique.

Significa que el valor de sus atributos **no** puede **ni debe** cambiar, por lo que quedarán fijos sin sufrir alteraciones. En caso de querer modificar algún valor de estos, habría un error de compilación.

---

> ¿Qué significa `this` en C++? ¿Dónde se usa explícita o implícitamente?

Es un puntero que mantiene la dirección de memoria del objeto actual, es decir, aquel usado para llamar al método. Se usa explícitamente para diferenciar ambigüedades cuando aparecen variables o parámetros con el mismo nombre que algún atributo de la clase o para retornar una referencia al mismo; implícitamente, al usar atributos del objeto.

---

> Explicar constructor move. Ejemplificar.
> Explique qué es y para qué sirve un **constructor MOVE** en C++. Indique cómo se comporta el sistema si éste **no es definido por el desarrollador**.

El constructor move es un constructor que a diferencia de aquel por copia, este mueve el objeto entero, cambiando el ownership del mismo a la clase construida. Es útil cuando se tienen objetos grandes para evitar copias en memoria, o cuando hay recursos como punteros a memoria dinámica.
Este tipo de constructor es eficiente ya que evita copias innecesarias y además previene leaks de memoria al transferir el ownership del objeto.

```cpp
class UnaClase {
 public:
    UnaClase(UnaClase&& otraClase) {
        // Lógica de construcción.
    }
};
```

El parámetro con el doble ampersand (`&&`) indica que debe utilizarse move semantics al pasar valores usando este constructor.

Si hay un constructor por copia, el compilador no genera un constructor por movimiento por defecto; si no hay un constructor por copia, se generará un constructor move implícitamente que hace una copia miembro a miembro, lo que es menos eficiente que uno definido a mano.

---

> ¿Qué es un **functor**? ¿Qué ventaja ofrece frente a una función convencional? **Ejemplifique**.

Un functor, u objeto función, es un objeto de una clase que sobrecarga el operador `()` y puede ser invocado como si fuera una función pasándole parámetros y retornando un resultado.

Ventajas:

1. Pueden tener estado: almacenan datos y modifican su comportamiento en función a estos.
2. Pueden ser asignados a variables, o bien pasados como argumentos.
3. Se pueden personalizar y acoplar a los datos.

```cpp
class OrdenarPorModulo {
public:
    bool operator()(int a, int b) {
        return abs(a) < abs(b);
    }
};

int main() {
    std::vector<int> numeros = {3, -2, 5, -1, 4};
    OrdenarPorModulo ordenar;
    std::sort(numeros.begin(), numeros.end(), ordenar);

    for (int num : numeros) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
}
```

En el ejemplo, OrdenarPorModulo es un functor y se usa en la función sort.

---

> ¿En qué consiste el patrón de diseño **RAII**? Ejemplifique.

El patrón de diseño RAII, Resource Acquisition Is Initialization, vincula la vida de los recursos a la del objeto: **cuando un objeto es creado adquiere el recurso; cuando se destruye el objeto, el recurso es liberado**. Esto ayuda a prevenir leaks de memoria.

```cpp
class GameController {
private:
    Serializer& serializer;
    Deserializer& deserializer;
    std::shared_ptr<Queue<std::unique_ptr<DTO>>>& gameQueue;

public:
    GameController(Serializer& serializer, Deserializer& deserializer,
                   std::shared_ptr<Queue<std::unique_ptr<DTO>>>& gameQueue);
};

GameController::GameController(Serializer& serializer, Deserializer& deserializer,
                               std::shared_ptr<Queue<std::unique_ptr<DTO>>>& gameQueue):
        serializer(serializer), deserializer(deserializer), gameQueue(gameQueue) {}
```

En el ejemplo anterior, al crear el constructor de GameController, este objeto adquiere las referencias a Serializer, Deserializer y al puntero compartido gameQueue.

---

## Compilación y Enlace

> ¿Qué es una **macro** de **C**? Detalle las **buenas prácticas** para su definición. Ejemplifique.

Una macro es una instrucción al procesador que permite reemplazar una secuencia de texto por otra durante la compilación.
Una buena práctica es que las constantes sean definidas por esta vía así se evita tener que cambiar su valor en varios lugares del código en lugar de hacerlo en un solo lugar.

```c
#define PI 3.141592652589
#define CUADRADO(x) x*x

int main(int argc, char* argv[]) {
    float radio = argv[1];
    float areaCirculo = PI * CUADRADO(radio);
}
```

---

> Describa el **proceso de transformación de código** fuente a un ejecutable. Precise las **etapas**, las **tareas desarrolladas** y los **tipos de error** generados en cada una de ellas.

---

> Explique qué se entiende por "**compilación condicional**". ¿En qué etapa del proceso de transformación de código se resuelve? **Ejemnplifique** mediante código C dando un caso de uso útil.

C++ ofrece la posibilidad de compilación condicional mediante la inclusión de ciertas directivas que controlan el comportamiento del preprocesador, de forma tal que este puede ignorar o compilar determinadas líneas de código en función de ciertas condiciones que son evaluadas durante el preprocesamiento. Las directivas #ifndef, #define, #endif, etc funcionan análogamente a los condicionales. En este ejemplo, si ifndef es positivo, no se redefine:

```c
#ifndef ALGO_H_
#define ALGO_H_
...
#endif // ALGO_H_
```

---

> **¿Qué características debe tener un compilador C para que sea considerado "portable"?**

Un compilador es portable si es capaz de generar código ejecutable para diferentes plataformas y sistemas operativos, minimizanod los cambios necesarios en el código fuente.

1. Adherencia estricta a estándares: ANSI C y extensiones específicas.
2. Gestión de diferencias entre plataformas: syscalls, tipos de datos, endianness.
3. Biblioteca estándar.
4. Opciones de compilación.
5. Documentación.

Ejemplos de compiladores portables: GCC, CLang.

---

> Explique **qué es** cada uno de los siguientes, haciendo referencia a su **inicialización**, su **comportamiento** y el **área de memoria** donde residen:
>
> - Una variable **global static**.
> - Una variable **local static**.
> - Un **atributo de clase static**.

Una variable global estática es una variable declarada fuera de cualquier función y tiene alcance de todo el programa. Al ser declarada static, mantiene su valor entre distintas llamadas a funciones.
Se inicializa explícitamente, o bien con 0 para tipos numéricos y null para punteros.
Tiene duración hasta el fin del programa y es accesible desde cualquier parte del código. Tiene enlace externo, puede ser referenciada desde otros archivos.
Vive en el Data Segment.

Una variable local static es una variable declarada dentro de una función, su alcance está dentro de esta misma, pero como es static, dura durante la ejecución del programa y no se destruye al terminar de ejecutar la función.
Se inicializa explícitamente o implícitamente (0 para tipos numéricos y null para punteros).
Dura hasta el final del programa y es accesible solo dentro de la función que lo declara. Tiene enlace interno, es visible dentro del archivo donde está definido.
Vive en el Data Segment.

Un atributo de clase estático es una variable miembro de una clase que pertenece a la clase y no a la instancia. Todas las instancias comparten esa misma copia de la variable.
Se inicializa fuera de la clase en el .cpp, dura durante toda la ejecución del programa y es accesible desde cualquier lado del código usando el nombre de la clase.
Vive en el Data Segment.

---

> Explique el funcionamiento de la sentencia de precompilación `#undef`.

La secuencia de precompilación `#undef` elimina la definición previa de algún símbolo de precompilación. Si no encuentra ningún símbolo con dicho nombre, no realiza ninguna acción.

---

> ¿Cómo se evita que un .h sea compilado **más de una vez** dentro de la misma unidad? ¿Qué **instrucciones de precompilación** suelen utilizarse? Ejemplifique.

Se suelen usar las instrucciones `#ifndef`, `#define` y `#endif`. Estas directivas comprueban la presencia o ausencia de los identificadores definidos.

```c
#ifndef MI_CLASE_H_
#define MI_CLASE_H_
...
#endif // MI_CLASE_H_
```

---

> ¿Qué es el pre-compilador? Explique dos instrucciones cualquiera que éste procese.

Es un procesador de texto que identifica MACROS y hace reemplazo de texto o algunas ediciones condicionales. Sirve para definir secuencias de código que se repetirán pero no tienen sentido como función o definir constantes.
Una sentencia usada es el `#define`, que define un símbolo para el precompilador que perdurará durante la compilación, por lo que se podrá consultar si ya se encuentra definido.
Otra sentencia usada es el `#include`, que busca el archivo indicado y lo copia en la fuente actual. Se puede usar para llamar a bibliotecas, secuencias, funciones y objetos dentro o fuera del programa.

---

> ¿En qué consiste el proceso de linkedición?

Consiste en asociar los símbolos procesaddos por el compilador con la dirección de memoria correspondiente. Por ejemplo, los métodos con sus definiciones, las funciones con sus bloques de memoria correspondiente, las variables globales con sus posiciones de memoria.

---

> **Describa y ejemplifique** el uso de la siguiente instrucción de precompilación: **#if**.

La instrucción de precompilación `#if` abre una compilación condicional, donde el código solo se compila si la expresión correspondiente al símbolo es verdadera.
En el código de ejemplo, DEBUG es verdadero puesto que está definido en 1, mientras que TESTING es falso ya que se define como 0. Por lo tanto, sí se imprimirá "DEBUGGING" pero no "TESTING".

```cpp
#define DEBUG 1
#define TESTING 0

#if DEBUG
    std::cout << "DEBUGGING" << std::endl;
#endif
#if TESTING
    std::cout << "TESTING" << std::endl;
#endif
```

---

## Declaraciones y Definiciones

> Describir y decir si es una declaración/definición:
>
> - `static int *(*A)();`
> - `extern unsigned char *(*B)[1];`
> - `bool *C() {return 0;}`

---

> **Describa con exactitud** las siguientes **declaraciones/definiciones globales**:
>
> 1. *`void (*F)(int i);`*
> 2. *`static void B(float a, float b) {}`*
> 3. *`int *(*C)[5];`*

---

> **Describa con exactitud** las siguientes **declaraciones/definiciones globales**:
>
> - `extern float (*l)[3];`
> - `static int *C[3];`
> - `static short F(const float *a);`

---

> Describa con exactitud cada uno de los siguientes:
>
> - **`static int A=7;`**
> - **`extern char *(*B)[7];`**
> - **`float **C;`**

---

> Escriba las siguientes definiciones/declaraciones:
>
> - Definición de una la función SUMA, que tome dos enteros largos con signo y devuelva su suma. Esta función solo debe ser visible en el módulo donde se la define.
> - Declaración de un puntero a puntero a entero sin signo.
> - Definición de un caracter solamente visible en el módulo donde se define.

---

> Escriba las siguientes definiciones/declaraciones:
>
> - Declaración de un puntero a puntero a entero largo con signo.
> - Definición de una la función RESTA, que tome dos enteros largos con signo y devuelva su resta. Esta función debe ser visible en todos los módulos del programa.
> - Definición de un caracter solamente visible en el módulo donde se define.

---

## Explicación de funciones

> Explique breve y concretamente qué es **f**:
>
> ```cpp
> int (*f) (short *, char[4]);
> ```

---

> Explique breve y concretamente qué es **f**:
>
> ```cpp
> int (*f) (short int*, char[3]);
> ```

---

> Explique breve y concretamente qué es **f**:
>
> `char (*f)(int *, char[3]);`

---

## Programación Orientada a Objetos

> Explicar "code bloat" en la construcción de un Template. Ejemplificar y mostrar cómo evitarlo.

El *code bloat* se produce cuando un Template termina siendo muy grande y teniendo demasiadas responsabilidades, normalmente también teniendo código duplicado.[^1]

[^1]: <https://refactoring.guru/refactoring/smells/bloaters>

Esto hace que la clase pierda eficiencia y sea difícil de mantener.

En este fragmento, se tiene una clase que realiza demasiadas acciones, pudiéndose modularizar.

```cpp
#include <iostream>
#include <vector>

template <typename T>
class DataProcessor {
public:
    void addData(const T& data) {
        data_.push_back(data);
    }

    void processData() {
        for (const auto& item : data_) {
            std::cout << item << std::endl;
        }
    }

    void clearData() {
        data_.clear();
    }

    void sortData() {
        std::sort(data_.begin(), data_.end());
    }

    T getMax() {
        return *std::max_element(data_.begin(), data_.end());
    }

    T getMin() {
        return *std::min_element(data_.begin(), data_.end());
    }

private:
    std::vector<T> data_;
};
```

Algunas maneras de evitarlo son el uso de la especialización, composición, interfaces o clases abstractas.

El fragmento anterior se puede mejorar:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

template <typename T>
class DataContainer {
public:
    void add(const T& data) {
        data_.push_back(data);
    }

    const std::vector<T>& getData() const {
        return data_;
    }

private:
    std::vector<T> data_;
};

template <typename T>
class DataProcessor {
public:
    void print(const DataContainer<T>& container) const {
        for (const auto& item : container.getData()) {
            std::cout << item << std::endl;
        }
    }

    T findMax(const DataContainer<T>& container) const {
        return *std::max_element(container.getData().begin(), container.getData().end());
    }

    // Otras operaciones
};
```

En la versión mejorada, el contenedor de datos se encarga de agregar datos y retornar los datos correspondientes; mientras que el procesador se encarga de realizar las operaciones de procesamiento.

---

> Explique qué son **los métodos virtuales** y para qué sirven. Dé un breve ejemplo donde su uso sea imprescindible.

Que un método sea virtual quiere decir que este puede variar su comportamiento según el tipo real del objeto aunque esté siendo accedido desde una clase base. Sirve especialmente para el polimorfismo donde los métodos que se indican virtuales serán aquellos que una clase que herede de aquella clase base podrá sobreescribir.

Un caso en donde es imprescindible el uso de métodos virtuales es en el destructor de una clase polimórfica: si no es virtual, al destruir una clase hija primero accede al destructor de la clase padre y destruirá la clase, y no ejecutará el destructor de la clase hija. Esto puede hacer que las partes no comunes entre los dos objetos no se destruyan, ocacionando un leak de memoria.

---

> Explique qué son **los métodos virtuales puros** y para qué sirven. Dé un breve ejemplo donde su uso sea **imprescindible**.

Un método virtual puro es un método que no tiene implementación en dicha clase. Es una promesa de que las clases derivadas implementarán dicho método.

Su uso es imprescindible, por ejemplo, en las clases abstractas, donde se debe declarar un método virtual puro para que no pueda ser instanciada.

---

> ¿Por qué las librerías que usan **Templates** se publican con todo el código fuente y no como un .h y .o/.obj?

Porque los templates dependen de un tipo de dato genérico y son instanciados en tiempo de compilación, debiéndose saber en dicho momento cuál es el tipo que se va a usar, ya que esto depende del contexto en el que es usado.

---

> ¿Cuál es el motivo por el cual las clases que utilizan templates se declaran y definen en los .h?

Los template deben poseer la definición en el mismo .h porque el compilador los usa para generar la clase actual, y este necesita el código para instanciar la plantilla. Si se colocara una implementación de plantilla en un archivo .cpp, solo podrá crear una instancia de la plantilla en ese mismo archivo .cpp.

---

> ¿Qué es el **polimorfismo**? Ejemplifique mediante código.

El polimorfismo es la capacidad de que un objeto se comporte de distintas formas según el contexto en el que se use.
Permite que el código sea flexible y además facilita la creación de jerarquía de clases y, bien implementado, simplifica el mantenimiento del código.

```cpp
class DTO {
 public:
    int type;
    virtual int getType();
    virtual ~DTO();
};

class UnDTOEspecifico : public DTO {
 public:
    int getType() override { return 0; }
};

class OtroDTOEspecifico : public DTO {
 public:
    int getType() override { return 1; }
};
```

---

> ¿Qué consideración particular debe tenerse en cuenta al momento de definir una clase que usa memoria dinámica y será usada con polimorfismo? ¿Por qué?

El destructor debe ser virtual. El objeto, si bien será una instancia de la clase derivada, será tratado como una instancia de clase base y, si su destructor no es virtual, al llamarlo no se liberará la memoria alocada por la clase derivada porque no se llamará al destructor de derivada.

---

## Análisis de Código

> Analice el siguiente código y determine lo que se imprime (valor de Pi)
>
> ```cpp
> main() {
>     int *Pi = 1000;
>     Pi++;
>     printf("Pi apunta a la dirección: %l", (long)Pi);
> }
> ```

El código intenta asignarle un entero a un puntero, por lo que tiene un comportamiento indefinido: la dirección de memoria 1000 puede ser (y probablemente sea) inválida. Lo que imprime dependerá de qué suceda aquí pero probablemente dé un error de compilación. Si imprime algo, será aleatorio o puede incluso no imprimir nada.

---

> Considere la estructura `struct ejemplo {int a; char b;}`. ¿Es verdad que **sizeof(ejemplo) = sizeof(a) + sizeof(b)**? **Justifique**.

No es necesariamente cierto que se dé dicha equidad. Esto se debe a que en muchos casos, dependiendo de la arquitectura, se agrega padding para alinear la memoria a múltiplos de un tamaño en específico, y de esta manera poder acceder a los elementos de la estructura sumando dicho valor, representando una mejora en el rendimiento permitiendo una mayor velocidad de acceso.
Si fuera el caso que se le introduce padding a la estructura ejemplo, y teniendo en cuenta que un int ocupa 4 bytes, y un char, 1, sizeof(ejemplo) daría 8 bytes, pues serían los 4 del int a, aquel del char b y 3 más de padding para alinear b.

---

> Indique la **salida** del siguiente programa:
>
> ```cpp
> class A {
>     A() {
>         cout << "A()" << endl;
>     }
>     ~A() {
>         cout << "~A()" << endl;
>     }
> }
>
> class B : public A {
>     B() {
>         cout << "B()" << endl;
>     }
>     ~B() {
>         cout << "~B()" << endl;
>     }
> }
>
> int main() {
>     B b;
>     return 0;
> }
> ```

Salida del programa:

```sh
A()
B()
~B()
~A()
```

B es una clase que hereda de A, por lo que cuando se instancia, el constructor primero llama al constructor de A, imprimiendo "A()", y luego se termina de construir B, y se imprime "B()". Luego, cuando se termina la ejecución, se destruye el objeto instanciado. Al ser B una clase que hereda de A, se llama primero al destructor de B, que imprime "~B()" y luego al destructor de la clase base A, que imprime "~A()".

---

> ¿Qué valor arroja **sizeof(int)**? **Justifique**.

Depende de la arquitectura del computador. Si tiene una arquitectura de 32 bits, por lo general el tamaño de int es de 4 bytes; si es de 64 bits, entonces puede ser de 4 u 8 bytes.
También depende del compilador, que puede asignar un tamaño mayor debido a ciertas optimizaciones que puede tener.

---

> Explique **qué es (a), (b), (c) y (d)**, haciendo referencia a su valor y momento de **inicialización**, su **comportamiento** y el **área de memoria** donde residen:
>
> ```cpp
> static int a;
> int b() {
>     static int c;
>     char d = 65;
>     return c + (int) d;
> }
> ```

- (a) es una variable global estática, vive en el data segment y se inicializa automáticamente con valor 0 al inicio del programa. Permanece en memoria hasta el final de ejecición y es accesible desde cualquier parte del código.

- (b) es una función que retorna un entero. No se inicializa, puesto que es una función. Se ejecuta cuando es llamada y devuelve el valor de c+d como entero. Vive en el code segment.

- (c) es una variable local estática, vive en el data segment y se inicializa automáticamente con valor 0 la primera vez que se ejecuta la función (b). Permanece en memoria desde que es inicializada hasta el final de ejecución del programa, ya que al ser estática no se destruye al final de la ejecución de (b).

- (d) es una variable local de la función (b) inicializada explícitamente con el valor 65. Existe dentro de (b), es accesible solamente dentro de esta función, y cuando esta finaliza ejecución, es destruida. Vive en el stack.

---

## Programación Orientada a Eventos

> **Describa** el concepto de **loop de eventos (events loop)** utilizando en programación orientada a eventos y, en particular, en entornos de interfaz gráfica (GUIs).

Un loop de eventos en programación orientada a eventos es aquel hilo o proceso que se encarga de detectar los eventos que suceden para poder procesarlos inmediatamente.
Consiste en su inicialización, donde se crea una cola que recibirá los eventos a procesar; luego espera a que la cola se llene con algo, y cuando se pushea algún evento, el loop se encarga de poppearlo y procesarlo. Una vez procesado, se vuelve a la etapa de espera donde aguarda por la llegada de un nuevo evento.
Los eventos pueden ser clicks, apretar alguna tecla, etc. Este loop es responsivo, ya que procesa cada comando apenas llega -el pop de la cola es bloqueante- y por ende también eficiente, ya que al ejecutarse en varios hilos/procesos (siempre y cuando se haga de esta forma y la computadora lo permita) permite la optimización y el mejor manejo de los recursos usando una estructura clara y organizada.

---

## Redes y Concurrencia - Threads

> ¿Qué es la critical section? Ejemplificar cómo protegerla.

En programación concurrente, una critical section es un bloque de código que accede a recursos compartidos y que se debe ejecutar de a un hilo por vez. Esto se debe a que si múltiples hilos accedieran en simultáneo se producirían race conditions, resultando en la obtención de resultados inesperados o incluso corrupción de datos.

Se puede proteger con el uso de mutex, ya que este restringe el acceso a un hilo y otro hilo podrá acceder a esta sección recién cuando el hilo que estuvo accediendo libere el mutex.

```cpp
#include <mutex>

int contador = 0;
std::mutex mtx;
void sumarUno() {
    std::lock_guard<std::mutex> lock(mtx);
    contador++;
}
```

---

> ¿Qué función se usa para lanzar un hilo? Ejemplificar.

En C++, para lanzar un hilo, se usa el operador `()`. Al instanciar un hilo de la librería estándar, se debe pasar el nombre del proceso que debe ejecutar el hilo:

```cpp
void printeaAlgo() {
    std::cout << "Algo" << std::endl;
}

int main() {
    std::thread hilo1(printeaAlgo);
    std::thread hilo2(printeaAlgo);
    printeaAlgo();
    hilo1.join();
    hilo2.join();
    return 0;
}
```

---

> ¿Cómo se logra que 2 **threads** accedan (lectura/escritura) a un mismo recurso compartido sin que se generen problemas de consistencia? **Ejemplifique**.

Para que 2 threads accedan a un mismo recurso compartido sin que se generen problemas de consistencia se debe proteger a la sección crítica, el bloque de código en el que se accede al recurso compartido, a través del uso de un mutex.

```cpp
#include <mutex>

int contador = 0;
std::mutex mtx;
void sumarUno() {
    std::lock_guard<std::mutex> lock(mtx);
    contador++;
}
```

Otra opción es usar variables atómicas, como por ejemplo `std::atomic<bool>`, usado en los trabajos prácticos propuestos por el curso para actualizar si se cerró el socket o no.

---

> ¿Qué ventaja ofrece un **lock raii** frente al tradicional **lock/unlock**?

Un lock RAII es mucho más fácil de usar, ya que el manejo de excepciones está incluido y además evita fugas. También evita que se generen deadlocks: con un lock RAII el mutex se libera cuando finaliza el scope del mismo, mientras que si no se realiza un unlock, fácilmente olvidable, en el método tradicional se quedaría estancado porque nunca se libera el mutex.

---

> ¿Qué función utiliza para esperar la terminación de un **thread**? Ejemplifique mediante código.

Cuando un thread se quiere cerrar o termina su vida útil, se usa la función `join()`.

```cpp
void doSomething(int num) {
    std::cout << "Hilo " << num << " comienza ejecución." << std::endl;
    // Hace algo
    std::cout << "Hilo " << num << " finaliza ejecución." << std::endl;
}

int main() {
    std::thread thread1(doSomething, 1);
    std::thread thread2(doSomething, 2);

    thread1.join();
    thread2.join();

    return 0;
}
```

---

> ¿Qué es un **Deadlock**? **Ejemplifique**.

Un deadlock es una situación en donde múltiples hilos se encuentran bloqueados porque están esperando la liberación de algún recurso que otro de los hilos bloqueados tiene, por lo que ninguno continúa su ejecución.

Se puede dar en una situación donde el hilo 1 accede al recurso A y luego intenta acceder al recurso B, sin liberar el mutex aún, mientras que un hilo 2 accedió al recurso B y tiene el mutex, y quiere acceder al recurso A. Este es un caso en donde los recursos no están asignados adecuadamente.

Los deadlocks se previenen usando una asignación de recursos adecuada y con mutex para evitar accesos concurrentes a las secciones críticas.

---

> Explique el propísito y uso de la función `BIND`.

La función `BIND` es una syscall que permite establecer una conexión entre un socket y una dirección IP a través de un puerto. En sockets, el uso de la función es previo al de la función `accept` ya que la misma prepara al socket para poder escuchar conexiones. `bind` establece a qué interfaz, IP y puerto se quiere asociar al socket. La dirección usada en la función `bind` son el resultado de la función `getaddrinfo`.

---

> ¿Para qué sirve la función `htons`? ¿A qué se debe su existencia?

Transforma el orden de los bytes de un short recibido por parámetro del endianness usado por el host al endianness de la red (big endian). Su existencia se debe a que la comunicación debe funcionar entre máquinas de distinto tipo, con distinta arquitectura y diferentes endianness, permitiendo trabajar con una representación común. La función `htons` asegura que los números se almacenen en memoria en el orden de bytes de la red, con el más significativo primero.

---

> Explique el propósito del llamado a **bind** y **accept** en una aplicación servidor **TCP/IP**.

Bind fue pensada para asignarle a un socket abierto una dirección. El uso de la función es previo al uso de la función accept, prepara al socket para poder escuchar conexiones.
Luego de hacer bind e iniciar el socket con listen para escuchar conexiones, la llamada accept bloquea al hilo de ejecución actual hasta obtener una conexión entrante al socket, devolviendo un nuevo file descriptor socket con el que se puede invocar un send o receive.

---

> ¿Cuál es el uso de la función `listen`? ¿Qué parámetros tiene y para qué sirven?

El listen define cuántas conexiones en espera pueden esperar hasta ser aceptadas. No limita cuántas conexiones totales puede haber.
Indica que el servidor está listo para recibir peticiones marcando el socket como pasivo.
Los parámetros que lleva son el socket y cuántas conexiones como máximo todavía puede aceptar (el backlog).

---

> ¿Puede la función `pthread_join` lockear algún hilo? Justifique.

La función `pthread_join` espera a que el hilo se detenga antes de realizar alguna acción. Si el objeto nunca fue detenido o se encuentra en alguna llamada bloqueante, puede generarle un lock al hilo que haya intentado sincronizar.

---

> Explique en qué situaciones es recomendable utilizar programación **multi-hilo** (**multithreading**) para realizar cierto procesamiento. ¿Existe algún caso donde utilizar multithreading sea perjudicial?

Las clases de sincronización se usan cuando se debe controlar el acceso a un recurso para garantizar la integridad del mismo. Las clases de acceso dee sincronización se usan para acceder a estos recursos controlados. El multithreadding permite a la CPU procesar varias tareas simultáneamente.
**Es perjudicial si sólo se tiene un núcleo de CPU sin bloqueo/espera en ningún subproceso**. Necesariamente hay una pérdida de tiempo de CPU para coordinar subprocesos múltiples. Dado que solo se puede ejecutar un subproceso, aunque todos podrían hacerlo (y por eso la condición de no bloqueo/no espera), el tiempo total es mayor.

---

> ¿Cuál es la diferencia entre un hilo y un proceso?

Un hilo es una unidad de ejecución dentro de un proceso, mientras que un proceso es un programa en sí.
El proceso tiene recursos del sistema y espacio de direcciones propio y es independiente de otros procesos, mientras que los hilos comparten recursos y espacio de direcciones y son interdependientes unos a otros.
Los hilos se usan cuando se pueden ejecutar concurrentemente múltiples tareas, mientras que los procesos son para cuando se desea aislar la tarea.

---

## Ejercicios de Código

[Doc resueltos](https://docs.google.com/document/d/1AvyV601tZKVNuATf2JEO7VyafU0iJQQk4DxBnX_MJ0k/edit?usp=sharing
)
[Carpeta resueltos](https://drive.google.com/drive/folders/1dH3v6_EqOJQtOQ2I2Wn5-2lhnv0q_I4Z?usp=drive_link)

### Sockets

> Escribir un programa que reciba el puerto, escuche una conexión, reciba paquetes terminados en '\0' y envíe el largo del paquete recibido. Si recibe un paquete de menos de 10 caracteres debe finalizar el programa liberando recursos.

---

> Escriba un **programa** (desde la inicialización hasta la liberación de recursos) que **reciba pauetes de la forma nnn+nn+...+nnnn=** (números separados por +, seguidos de =) e **imprima el resultado de la suma de cada paquete** por pantalla. Al recibir un **paquete vacío ("=") debe cerrarse orenadamente**. No considere errores.

---

> Escriba un programa que reciba por **línea de comandos** un **Puerto** y una **IP**. El programa debe establecer una única conexión, quedar en escucha e **imprimir en stdout todo** lo recibido. Al recibir el texto 'FINAL' **debe finalizar** el programa **sin imprimir dicho texto**.

---

> Escriba un programa que reciba por **línea de comandos un Puerto y una IP**. El programa debe aceptar una **única conexión e imprimir en stdout la sumatoria de los enteros recibidos en cada paquete**. Un paquete está definido como una **sucesión de números recibidos como texto, en decimal, separados por comas y terminado con un signo igual** (ej, "27,12,32="). Al recibir el texto **'FIN' debe finalizar el programa ordenadamente** liberando los recursos.

---

> Escriba un programa que reciba por **línea de comandos** un **Puerto** y una **IP**. El programa debe aceptar una **única conexión** y recibir paquetes (**cada paquete inicia con '[' y finalza con ']'**). Para cada paquete recibido debe **imprimir el checksum** (sumatoria de bytes del paquete) del mismo, excepto que el **paquete esté vacío** ('[]'), en cuyo caso **debe finalizar**.

---

#### Ejemplo Inicalización Socket

```c
int main(int argc, char* argv[]) {
    if (argc != 3) {
        // Normalmente recibe puerto e ip.
        return -1;
    }

    // Procesar parámetros
    char* port = argv[1];
    int ip = atoi(argv[2]);

    // Crear Socket
    int skt_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (skt_fd < 0) {
        perror("Error en creación del socket");
        return -1;
    }

    // Configurar la dirección del servidor
    struct sockaddr_in address;
    memset(&address, 0, sizeof(address));
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = inet_addr(argv[2]);
    address.sin_port = htons(atoi(argv[1]));

    // Asociar el socket a la dirección y puerto
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }

    // Escuchar conexiones
    if (listen(server_fd, 1) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }

    // Aceptar una conexión
    int new_socket;
    if ((new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&address))<0) {
        perror("accept");
        exit(EXIT_FAILURE);
    }

    // Procesar los paquetes.

    // Cerrar el socket
    close(new_socket);
    shutdown(server_fd, SHUT_RDWR);
    return 0;
}
```

---

### Estructuras de Datos - Operadores

> Declarar una clase que contenga: atributos, accesibilidad, operadores +, ++, --, float, <<, >> y constructor move.

---

> Escriba el **.H de una biblioteca** de funciones **ISO C** para cadenas de caracteres. Incluya, al menos, 4 funciones.

---

> Escriba el **.H de una biblioteca** de funciones **ISO C** para números complejos. Incluya, al menos, 4 funciones.

---

> **Declare** una clase de elección libre. Incluya todos los **campos de datos** requeridos con su **correcta exposición/publicación**, y los operadores ++, -, ==, >> (carga), << (impresión), constructor move y operador float().

---

> **Declare la clase Número**, para encapsular una cadena numérica larga. Incluya al menos: Constructor(unsigned long), Constructor default y Constructor move; operador <<, (), =, long y ++(int). Implemente el operador >>.

---

> **Declare** una clase a elección considerando:
>
> - **Atributos** que son necesarios
> - **Accesibilidad** a la Clase
> - **Incluir** los operadores +, ++ (post-incremento), --(pre-incremento), >> (impresión), << (carga desde consola), long

---

> **Declare la clase TELEFONO** para encapsular una cadena numérica correspondiente a un teléfono. Incluya al menos: **Constructor(area, numero)**, **Constructor move** y constructor de **Copia**; **Operador <<, ==, =, long y >>**. Implemente el **operador >>**.

---

> **Declare** la clase IPv4 para almacenar una dirección IP. Incluya **constructor default, constructor move, constructor de copia**, y los siguientes operadores: **operator<<, operator==, operator= y operator+**.

---

#### Ejemplo Declaración Operadores de una Clase Genérica

```cpp
template <typename T>
class GenericClass {
public:
    // Operadores aritméticos binarios
    GenericClass operator+(const GenericClass& other) const;
    GenericClass operator-(const GenericClass& other) const;
    GenericClass operator*(const GenericClass& other) const;
    GenericClass operator/(const GenericClass& other) const;

    // Operadores de comparación
    bool operator==(const GenericClass& other) const;
    bool operator!=(const GenericClass& other) const;
    bool operator<(const GenericClass& other) const;
    bool operator>(const GenericClass& other) const;
    bool operator<=(const GenericClass& other) const;
    bool operator>=(const GenericClass& other) const;

    // Operadores lógicos bit a bit
    GenericClass operator&(const GenericClass& other) const;
    GenericClass operator|(const GenericClass& other) const;
    GenericClass operator^(const GenericClass& other) const;
    GenericClass operator~() const; // Complemento a uno

    // Operadores lógicos booleanos
    bool operator&&(const GenericClass& other) const;
    bool operator||(const GenericClass& other) const;

    // Operadores de asignación
    GenericClass& operator=(const GenericClass& other);
    GenericClass& operator+=(const GenericClass& other);
    GenericClass& operator-=(const GenericClass& other);
    // ... y otros operadores de asignación compuestos

    // Operadores de carga e impresión
    friend std::ostream& operator<<(std::ostream& os, const GenericClass& class);
    friend std::istream& operator>>(std::istream& os, const GenericClass& class);

    // Operadores de incremento y decremento
    GenericClass& operator++(); // Preincremento
    GenericClass operator++(int); // Postincremento
    GenericClass& operator--(); // Predecremento
    GenericClass operator--(int); // Postdecremento

    // Cast a int
    explicit operator int() const;
};
```

---

### Manejo de Archivos

> Escribir un programa ISO C que procese "nros11.bin" sobre sí mismo. El procesamiento consiste en leer números big endian en base 11 de 3 símbolos y duplicar los múltiplos de 13.

---

> Escribir **un programa ISO C** que procese el archivo de **enteros de 3 bytes big endian cuyo nombre es recibido como parámetro**. El procesamiento consiste en **eliminar los números múltiplos de 3**, trabajando sobre el mismo archivo (sin archivos intermedios ni en memoria).

---

> Escribir **un programa ISO C** que procese el archivo "nros2bytes.dat" **sobre sí mismo, duplicando los enteros de 2 butes múltiplos de 3**.

---

> Escribir **un programa ISO C** que reciba por argumento el nombre de un archivo de texto y lo procese sobre sí mismo (sin crear archivos intermedios ni subiendo todo su contenido a memoria). El procesamiento consiste en **eliminar las líneas de 1 sola palabra**.

---

> Escribir **un programa C** que procese el archivo "numeros.txt" sobre sí mismo (sin crear archivos intermedios y sin subir el archivo a memoria). El procesamiento consiste en leer grupos de 4 caracteres hexadecimales y reemplazarlos por los correspondientes dígitos decimales (que representen el mismo número leído pero en decimal).

---

> Escriba una **función ISO C** que permita **procesar un archivo texto que contenga frases (1 por línea) sobre sí mismo, sin generar archivos intermedios ni cargar el archivo completo a memoria**. El procesamiento consiste en eliminar las palabras de más de 3 letras de cada línea.

---

> Escribir **un programa ISO C** que procese el archivo "nros_2bytes_bigendian.dat" **sobre sí mismo, eliminando los números múltiplos de 7**.

---

> Escribir **un programa C** que procese el archivo "numeros.txt" sobre sí mismo (sin crear archivos intermedios y sin subir el archivo a memoria). El procesamiento consiste en leer nros hexadecimales de 4 símbolos y reemplazarlos por su valor decimal (en texto).

---

> Escribir **un programa ISO C** que procese el archivo "valuesword.dat" **sobre sí mismo, eliminando los words (2 bytes) múltiplos de 16**.

---

> Escriba una **función ISO C** que permita **procesar un archivo texto sobre sí mismo**, que contenga **una palabra por línea**. El procesamiento consiste en **ordenar las palabras (líneas) alfabéticamente** considerando que el archivo **no entra en memoria**.

---

> Escribir **programa ISO C** que procese el archivo de **texto cuyo nombre es recibido como parámetro**. El procesamiento consiste en **leer oraciones y suprimir aquellas que tengan más de 3 palabras**. Asuma que el archivo no puede cargarse en memoria, pero una oración sí puede.

---

#### Ejemplo funciones cambio de base

```c
// Base N, Múltiplos de X y números de S símbolos, asumiendo que son little endian
#define BASE N
#define MULTIPLO_DE X
#define CANT_SIMBOLOS S
int baseN_to_int(const char* num) {
    int res = 0;
    int base = 1;
    for (int i=(CANT_SIMBOLOS - 1); i >= 0; i--) {
        char digito = num[i];
        bool mayor_que_diez = (digito >= 'A');
        res += ((mator_que_diez) ? (digito - 'A' + 10) : (digito - '0')) * base;
        base *= BASE;
    }
    return res;
}

int int_to_baseN(const char* num) {
    int res = 0;
    for (int i = (CANT_SIMBOLOS - 1); i >= 0; i--) {
        int digito;
        if (num[i] >= '0' && num[i] <= '9') {
            digito = num[i] - '0';
        } else if (num[i] >= 'A' && num[i] <= MAX_BASE) {
            // MAX_BASE es la letra máxima que puede tener la base.
            // Ej.: 11 => A | 16 => F |  20 => J | 25 => O | 27 => Q | 32 => V | 36 => Z
            digito = num[i] - 'A' + 10;
        } else {
            // No sé cómo se hace.
            return -1;
        }
    }
}

// Para pasar de bases entre 2 y 36 a base 10
long enteroLong = strtol(puntero_a_numero_base_N, NULL, N);
int enteroInt = (int) enteroLong;
```

#### Ejemplo abrir un archivo, buscar posición y escribir

```c
int main(int argc, char* argv[]) {
    FILE *archivo;
    char name[] = "nombre_archivo.bin/dat"; // bin o dat porque uso r+b
    archivo = fopen(name, "r+b");
    if (archivo == NULL) return -1;

    // Leer X bytes:
    char var[X+1]; // Lo leído + '\0'
    fread(var, sizeof(char), X, archivo); // devuelve la cantidad de bytes leídos.
    // Moverse Y bytes desde el inicio del archivo
    fseek(archivo, Y, SEEK_SET);
    // Moverse P bytes hacia adelante desde el lugar actual
    fseek(archivo, P, SEEK_CUR);
    // Moverse P bytes hacia atrás desde el lugar actual
    fseek(archivo, -P, SEEK_CUR);
    // Moverse al final del archivo
    fseek(archivo, 0, SEEK_END);
    // Escribir
    fwrite(lo_que_sobreescribo, sizeof(char), bytes_de_lo_que_sobreescribo, archivo);
    // Escribir un solo char
    fputc('X', archivo):

    fclose(archivo);
    return 0;
}
```

---

### Funciones C++

> Implemente una función **C++** denominada **DobleSiNo** que reciba dos listas de elementos y devuelva una nueva lista **duplicando los elementos de la primera que no están en la segunda**:
>
> ```cpp
> std::list<T> DobleSiNo(std::list<T> a, std::list<T> b);
> ```

---

> Implemente una función **C++** denominada **Sacar** que reciba dos listas de elementos y devuelva una nueva lista con los elementos de la primera que no están en la segunda:
>
> ```cpp
> std::list<T> Sacar(std::list<T> a, std::list<T> b);
> ```

---

> Escriba **una función ISO C** llamada ***Replicar*** que reciba 1 cadena (**S**), dos índices (**I1 e I2**) y una cantidad (**Q**). La función debe retornar una copia de S salvo los caracteres que se encuentran entre los índices I1 e I2, que serán duplicados Q veces.
> Ej, replicar("Hola", 1, 2, 3) retorna "Hololola".

---

> Escriba **un programa C** que tome 2 cadenas por línea de comandos: A y B; e imprima la cadena A después de haber duplicado **todas las ocurrencias de B**.
>
> Ej: reemp.exe "Este es el final" final ---> Este es el final final.

---

> Implemente una función **C++** denominada **DobleSegunda** que reciba dos listas de elementos y devuelva una nueva lista duplicando los elementos de la primera si están en la segunda:
>
> ```cpp
> std::list<T> DobleSegunda(std::list<T> a, std::list<T> b);
> ```

```cpp
#include <list>

template <typename T>
std::list<T> dobleSegunda(std::list<T> a, std::list<T> b) {
    std::list<T> respuesta;

    for (const auto &T elem : a) {
        respuesta.push_back(elem);
        auto it = std::find(b.begin(), b.end(), elem);
        if (it != b.end()) {
            // En este caso, elem está tanto en A como en B. Lo pusheo de nuevo.
            respuesta.push_back(elem);
        }
    }

    return respuesta;
}
```

---

> **Implemente** la función **`void ValorHex(char *hex, int *ent)`** que interprete la cadena **hex** (de símbolos hexadecimales) y guarde el valor correspondiente en el entero indicado por **ent**.

---

> Implemente una función C++ denominada **Interseccion** que reciba dos listas de elementos y devuelva una nueva lista con los elementos que se encuentran en ambas listas:
>
> `std::list<T> Interseccion(std::list<T> a, std::list<T> b);`

---

> Escriba un programa (desde la inicialización hasta la liberación de recursos) que reciba **paquetes de texto delimitados por corchetes angulares ("<...>") y los imprima completos por pantalla**. Al recibir un paquete vacío ("<>") debe cerrarse ordenadamente. No considere errores.

---

> Implemente una función C++ denominada **Suprimir** que reciba dos listas de elementos y devuelva una nueva lista con elementos de la primera que no están en la segunda:

```cpp
std::list<T> Suprimir(std::list<T> a, std::list<T> b);
```

---

### Threads

> Escriba un programa que imprima por salida estándar los números entre 1 y 100, **en orden ascendente**. Se pide que los números sean **contabilizados por una variable global única** y que los **pares sean escritos por un hilo** mientras que los **impares sean escritos por otro**. **Contemple la correcta sincronización** entre hilos y la liberación de los recursos utilizados.

---

### Librerías gráficas

> Escriba una rutina que dibuje las dos diagonales de la pantalla en color rojo.

Con QT:

```cpp
#include <QApplication>
#include <QWidget>
#include <QPainter>

class DiagonalWindow : public QWidget {
public:
    DiagonalWindow(QWidget *parent = nullptr) : QWidget(parent) {}

protected:
    void paintEvent(QPaintEvent *event) override {
        QPainter painter(this);
        painter.setPen(Qt::red);

        // Obtenemos las dimensiones de la ventana
        int width = this->width();
        int height = this->height();

        // Dibujamos las diagonales
        painter.drawLine(0, 0, width, height); // Diagonal superior izquierda a inferior derecha
        painter.drawLine(width, 0, 0, height); // Diagonal superior derecha a inferior izquierda
    }
};

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    DiagonalWindow window;
    window.show();
    return app.exec();
}
```

Con SDL:

```cpp
#include <SDL2/SDL.h>

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        // Manejar errores
        return -1;
    }

    SDL_Window* window = SDL_CreateWindow("Diagonales Rojas", SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, 640, 480, SDL_WINDOW_SHOWN);
    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255); // Rojo

    // Obtener las dimensiones de la ventana
    int width, height;
    SDL_GetWindowSize(window, &width, &height);

    // Dibujar las diagonales
    SDL_RenderDrawLine(renderer, 0, 0, width, height);
    SDL_RenderDrawLine(renderer, width, 0, 0, height);

    SDL_RenderPresent(renderer);

    // Bucle principal (simplificado)
    SDL_Event event;
    while (true) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                goto out;
            }
        }
        SDL_Delay(16);
    }
out:
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
    return 0;
}
```

---

> **Escriba una rutina** (para ambiente gráfico Windows o Linux) que dibuje un **triángulo amarillo** del tamaño de la ventana.

Con QT:

```cpp
#include <QApplication>
#include <QWidget>
#include <QPainter>

class TrianguloAmarillo : public QWidget {
public:
    TrianguloAmarillo(QWidget *parent = nullptr) : QWidget(parent) {}

protected:
    void paintEvent(QPaintEvent *event) override {
        QPainter painter(this);
        painter.setBrush(Qt::yellow);

        // Obtenemos las dimensiones de la ventana
        int width = this->width();
        int height = this->height();

        // Calculamos los vértices del triángulo (ajusta según tus necesidades)
        QPointF points[3] = {
            QPointF(0, height),
            QPointF(width, height),
            QPointF(width / 2, 0)
        };

        painter.drawPolygon(points, 3);
    }
};

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    TrianguloAmarillo window;
    window.show();
    return app.exec();
}
```

Con SDL:

```cpp
#include <SDL2/SDL.h>

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        // Manejar errores
        return -1;
    }

    SDL_Window* window = SDL_CreateWindow("Triángulo Amarillo", SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, 640, 480, SDL_WINDOW_SHOWN);
    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    SDL_SetRenderDrawColor(renderer, 255, 255, 0, 255); // Amarillo
    SDL_RenderClear(renderer);

    // Dibujar el triángulo (ajusta los puntos según tus necesidades)
    SDL_Vertex vertices[] = {
        {100, 400, {255, 255, 0, 255}},
        {540, 400, {255, 255, 0, 255}},
        {320, 100, {255, 255, 0, 255}}
    };
    SDL_RenderGeometry(renderer, NULL, vertices, 3);

    SDL_RenderPresent(renderer);

    // Bucle principal (simplificado)
    SDL_Event event;
    while (true) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                goto out;
            }
        }
        SDL_Delay(16);
    }
out:
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
    return 0;
}
```
