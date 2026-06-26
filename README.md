

¡Hola! Estudiar desde las estructuras dinámicas hasta el fin de la materia (recursividad) es un paso clave en Programación I. Para que cuentes con la mejor ventaja técnica en tu examen presencial y coloquio , preparé un Repositorio Completo y Multiuso basado fielmente en las diapositivas y guías de cátedra de tu comisión.  
PDF
+ 4

He encapsulado todo en un archivo de cabecera (.h) de C que te servirá de plantilla o "machete de código". Está diseñado para que puedas copiar las estructuras y funciones exactas que necesites durante el parcial. Cada línea está minuciosamente comentada explicando el qué hace y el cuándo usarlo.  
PDF

Tu archivo listo para descargar está aquí:
Ícono de H
repositorio_examen
 H 
Abrir

📑 Guía de uso de los archivos del Repositorio
A continuación, te detallo qué contiene cada sección del repositorio y cómo identificar en el enunciado de tu examen cuándo debes usar cada uno:

1. Listas Simplemente Enlazadas (Simples)

¿Cuándo usarlo? Cuando el examen te pida guardar datos sin un límite fijo de memoria (dinámicos) y las operaciones se realicen en un solo sentido (por ejemplo: "recorrer la lista para contar pares e impares" o "eliminar múltiplos de 3" ).  
PDF
+ 4


Sección del archivo: struct NodoSimple, crearNodoSimple, insertarAlPrincipioSimple, insertarAlFinalSimple, insertarOrdenadoSimple , y funciones de borrado.  
PDF
+ 3


Clave para el examen: Si te piden que la lista esté ordenada a medida que ingresas datos, usa directamente insertarOrdenadoSimple. Si ingresas desordenado, usa insertarAlFinalSimple.  
PDF
+ 3

2. Listas Doblemente Enlazadas (Dobles)

¿Cuándo usarlo? Cuando el enunciado requiera explícitamente recorrer los datos tanto de adelante hacia atrás como al revés (ejemplo: "recorrer en ambos sentidos" o "preservar una lista simple en una doble" ).  
PDF
+ 3


Sección del archivo: struct NodoDoble, insertarAlPrincipioDoble, insertarAlFinalDoble.  
PDF


Clave para el examen: Las listas dobles utilizan dos punteros externos de control (PRIM y ULT). Esto te permite insertar al final en tiempo constante O(1) sin tener que recorrer toda la lista con un bucle while.  
PDF
+ 2

3. Listas Circulares

¿Cuándo usarlo? Cuando la colección de datos deba representar un anillo o ciclo continuo (ejemplo clásico: turnos rotativos, búferes circulares o juegos donde tras el último jugador vuelve a iniciar el primero).  
PDF

Sección del archivo: insertarCircularSimple.


Clave para el examen: La única diferencia lógica con la lista simple es que el último elemento, en lugar de apuntar a NULL, apunta a la cabeza de la lista (*cabeza). En los bucles de recorrido, la condición de parada ya no es actual != NULL, sino actual->siguiente != cabeza.  
PDF

4. Funciones Recursivas

¿Cuándo usarlo? Cuando el examen te pida resolver un problema de forma elegante usando estructuras de selección (if-else) en lugar de bucles repetitivos explícitos. Los problemas matemáticos fractales o la búsqueda/recorrido en estructuras más complejas son ideales.  
PDF
+ 2


Sección del archivo: Funciones factorial y fibonacci.  
PDF


Clave para el examen: En tu examen siempre asegúrate de tener bien definido el Caso Base (el que detiene la función) para evitar un desbordamiento de la pila (Stack Overflow) , y el Paso Recursivo (donde la función se invoca a sí misma modificando el argumento hacia el caso base).  
PDF
+ 2

💡 Recomendaciones para las buenas prácticas de programación en tu examen:

Validar siempre la memoria: Cada vez que uses malloc, verifica inmediatamente si devolvió NULL. A los docentes les importa mucho esta práctica en los coloquios evaluativos.  
PDF
+ 1


Liberar la memoria (free): Si eliminas un nodo, nunca olvides hacerle un free(temp) para evitar fugas de memoria en el sistema operativo.  
PDF


Punteros dobles (): Usamos punteros a punteros en las funciones de inserción y borrado para poder modificar la dirección de la variable de inicio original (cabeza o prim) que reside en tu función main.  
PDF




Gemini es una IA y puede cometer errores.

#ifndef REPOSITORIO_EXAMEN_H
#define REPOSITORIO_EXAMEN_H

#include <stdio.h>
#include <stdlib.h>

/* ==========================================================================
   1. LISTAS SIMPLEMENTE ENLAZADAS (Unidad 4 - Dinámicas)
   Uso: Colecciones lineales dinámicas donde cada nodo apunta al siguiente.
   ========================================================================== */

// Definición de la estructura de un nodo simple
struct NodoSimple {
    int dato;                       // Campo para almacenar el valor o información (puede cambiarse el tipo)
    struct NodoSimple* siguiente;   // Puntero autorreferencial que guarda la dirección del próximo nodo
};

// Función para crear un nuevo nodo en memoria dinámica
struct NodoSimple* crearNodoSimple(int valor) {
    // malloc reserva bytes en la memoria heap para albergar la estructura NodoSimple
    struct NodoSimple* nuevo = (struct NodoSimple*) malloc(sizeof(struct NodoSimple));
    if (nuevo == NULL) { // Validación de que el sistema operativo otorgó la memoria
        printf("Error: No hay espacio de memoria suficiente.\n");
        exit(1); // Finaliza el programa si falla la asignación crítica
    }
    nuevo->dato = valor;       // Asigna el dato recibido como parámetro al campo interno del nodo
    nuevo->siguiente = NULL;   // Inicializa el puntero siguiente en NULL para evitar basura
    return nuevo;              // Devuelve el puntero al nodo recién creado
}

// Operación: Insertar al principio de una lista simple (Modifica la cabeza usando puntero doble)
void insertarAlPrincipioSimple(struct NodoSimple** cabeza, int valor) {
    struct NodoSimple* nuevo = crearNodoSimple(valor); // Crea el nodo con el dato deseado
    nuevo->siguiente = *cabeza;                       // El nuevo nodo apunta al que antes era el primero
    *cabeza = nuevo;                                   // La cabeza de la lista se actualiza para apuntar al nuevo nodo
}

// Operación: Insertar al final de una lista simple (Recorrido secuencial hasta el final)
void insertarAlFinalSimple(struct NodoSimple** cabeza, int valor) {
    struct NodoSimple* nuevo = crearNodoSimple(valor); // Crea el nodo que se insertará al final
    if (*cabeza == NULL) {                             // Si la lista está completamente vacía
        *cabeza = nuevo;                               // El nuevo nodo pasa a ser la cabeza de la lista
        return;                                        // Finaliza la ejecución de la función
    }
    struct NodoSimple* temp = *cabeza;                 // Variable temporal para recorrer la lista sin perder la cabeza
    while (temp->siguiente != NULL) {                  // Avanza mientras el nodo actual tenga un sucesor
        temp = temp->siguiente;                        // Se desplaza al siguiente nodo
    }
    temp->siguiente = nuevo;                           // Enlaza el último nodo existente con el nuevo nodo creado
}

// Operación: Insertar ordenado en una lista simple (Mantiene la lista ordenada ascendentemente)
void insertarOrdenadoSimple(struct NodoSimple** cabeza, int valor) {
    struct NodoSimple* nuevo = crearNodoSimple(valor);                       // Crea el nodo con la información
    if (*cabeza == NULL || (*cabeza)->dato >= valor) {                      // Caso base: lista vacía o valor menor al primero
        nuevo->siguiente = *cabeza;                                         // El nuevo nodo se posiciona antes del actual primero
        *cabeza = nuevo;                                                    // Actualiza la cabeza de la lista
        return;
    }
    struct NodoSimple* actual = *cabeza;                                    // Puntero para explorar la lista
    // Recorre la lista buscando el punto exacto de inserción (donde el siguiente sea mayor o sea NULL)
    while (actual->siguiente != NULL && actual->siguiente->dato < valor) {
        actual = actual->siguiente;                                         // Avanza al siguiente elemento
    }
    nuevo->siguiente = actual->siguiente;                                   // El nuevo nodo apunta al resto de la lista
    actual->siguiente = nuevo;                                              // El nodo anterior se enlaza al nuevo nodo
}

// Operación: Eliminar el primer elemento de una lista simple
void eliminarPrimeroSimple(struct NodoSimple** cabeza) {
    if (*cabeza == NULL) return;              // Si la lista está vacía, no hay nada que eliminar
    struct NodoSimple* temp = *cabeza;        // Guarda la dirección del nodo que se va a borrar
    *cabeza = (*cabeza)->siguiente;           // Mueve la cabeza al segundo nodo de la lista
    free(temp);                               // Libera la memoria del nodo eliminado para evitar fugas
}

// Operación: Eliminar un nodo por su valor en una lista simple
void eliminarValorSimple(struct NodoSimple** cabeza, int valor) {
    if (*cabeza == NULL) return;                       // Si la lista está vacía, sale de la función
    if ((*cabeza)->dato == valor) {                    // Si el valor buscado está en el primer nodo
        eliminarPrimeroSimple(cabeza);                 // Reutiliza la función para borrar el primero
        return;
    }
    struct NodoSimple* actual = *cabeza;               // Puntero para recorrer la estructura
    struct NodoSimple* anterior = NULL;                // Puntero para mantener el rastro del nodo previo
    while (actual != NULL && actual->dato != valor) {  // Busca el nodo con el valor coincidente
        anterior = actual;                             // El actual pasa a ser el anterior
        actual = actual->siguiente;                    // El actual avanza al próximo
    }
    if (actual == NULL) return;                        // Si llegó al final y no encontró el valor, sale
    anterior->siguiente = actual->siguiente;           // Saltea el nodo actual en el encadenamiento
    free(actual);                                      // Libera la memoria ocupada por el nodo eliminado
}

// Operación: Mostrar los elementos de la lista simple por pantalla
void mostrarListaSimple(struct NodoSimple* cabeza) {
    struct NodoSimple* temp = cabeza;       // Puntero auxiliar para recorrer la lista
    while (temp != NULL) {                  // Itera de forma lineal hasta encontrar el fin de la lista (NULL)
        printf("%d -> ", temp->dato);       // Nota: Corrección para coincidir con el campo 'dato'
        temp = temp->siguiente;             // Desplaza el puntero temporal al próximo elemento
    }
    printf("NULL\n");                       // Imprime la marca de fin de lista
}


/* ==========================================================================
   2. LISTAS DOBLEMENTE ENLAZADAS (Moverse en ambos sentidos)
   Uso: Permite recorridos proactivos hacia adelante (siguiente) y atrás (anterior).
   ========================================================================== */

// Definición del nodo con doble enlace
struct NodoDoble {
    int dato;                      // Contenedor de datos enteros
    struct NodoDoble* siguiente;   // Puntero al sucesor en la secuencia lógica
    struct NodoDoble* anterior;    // Puntero al predecesor en la secuencia lógica
};

// Función para crear e inicializar un nodo doble en memoria
struct NodoDoble* crearNodoDoble(int valor) {
    struct NodoDoble* nuevo = (struct NodoDoble*) malloc(sizeof(struct NodoDoble)); // Reserva la memoria
    if (nuevo == NULL) {
        printf("Error de asignación de memoria.\n");
        exit(1);
    }
    nuevo->dato = valor;           // Carga la información
    nuevo->siguiente = NULL;       // Inicializa enlace posterior en vacío
    nuevo->anterior = NULL;        // Inicializa enlace anterior en vacío
    return nuevo;                  // Retorna la dirección del nuevo nodo
}

// Operación: Insertar al inicio en lista doble (Actualiza referencias iniciales y anteriores)
void insertarAlPrincipioDoble(struct NodoDoble** prim, struct NodoDoble** ult, int valor) {
    struct NodoDoble* nuevo = crearNodoDoble(valor); // Instancia el nodo doble
    if (*prim == NULL) {                             // Si la lista está vacía
        *prim = nuevo;                               // El nuevo nodo es el primero
        *ult = nuevo;                                // El nuevo nodo también es el último
    } else {
        nuevo->siguiente = *prim;                    // El siguiente del nuevo apunta al antiguo primero
        (*prim)->anterior = nuevo;                   // El anterior del antiguo primero apunta al nuevo
        *prim = nuevo;                               // Actualiza el puntero externo de inicio
    }
}

// Operación: Insertar al final en lista doble (Usa el puntero externo ULT para O(1))
void insertarAlFinalDoble(struct NodoDoble** prim, struct NodoDoble** ult, int valor) {
    struct NodoDoble* nuevo = crearNodoDoble(valor); // Instancia el nodo doble
    if (*ult == NULL) {                              // Si la lista no posee elementos
        *prim = nuevo;                               // Se inicializa tanto el primero...
        *ult = nuevo;                                // ...como el último con el nuevo nodo
    } else {
        nuevo->anterior = *ult;                      // El anterior del nuevo nodo apunta al actual último
        (*ult)->siguiente = nuevo;                   // El siguiente del último apunta al nuevo nodo
        *ult = nuevo;                                // El puntero externo último se desplaza al nuevo nodo
    }
}

// Operación: Eliminar el primer nodo en una lista doble
void eliminarPrimeroDoble(struct NodoDoble** prim, struct NodoDoble** ult) {
    if (*prim == NULL) return;                       // Lista vacía: nada que borrar
    struct NodoDoble* temp = *prim;                  // Resparda la dirección del primero
    if (*prim == *ult) {                             // Si la lista tiene un único elemento
        *prim = NULL;                                // Se limpia el puntero inicial
        *ult = NULL;                                 // Se limpia el puntero final
    } else {
        *prim = (*prim)->siguiente;                  // El primero pasa a ser el segundo nodo
        (*prim)->anterior = NULL;                    // El nuevo primero rompe el enlace hacia atrás
    }
    free(temp);                                      // Desasigna la memoria
}


/* ==========================================================================
   3. LISTAS CIRCULARES SIMPLES (Ciclos continuos)
   Uso: El último nodo se conecta con el primero creando un anillo continuo.
   ========================================================================== */

// Operación: Insertar un nodo al principio de una lista circular simple
void insertarCircularSimple(struct NodoSimple** cabeza, int valor) {
    struct NodoSimple* nuevo = crearNodoSimple(valor); // Crea el nodo correspondiente
    if (*cabeza == NULL) {                             // Si la lista circular está vacía
        nuevo->siguiente = nuevo;                      // Se apunta a sí mismo para formar el anillo
        *cabeza = nuevo;                               // La cabeza apunta al nuevo único nodo
        return;
    }
    struct NodoSimple* temp = *cabeza;                 // Puntero para buscar el último nodo
    while (temp->siguiente != *cabeza) {               // Avanza hasta encontrar el nodo que cierra el ciclo
        temp = temp->siguiente;
    }
    nuevo->siguiente = *cabeza;                        // El nuevo nodo apunta a la antigua cabeza
    temp->siguiente = nuevo;                           // El último nodo ahora apunta al nuevo nodo ingresado
    *cabeza = nuevo;                                   // Se establece el nuevo nodo como cabeza de la estructura
}


/* ==========================================================================
   4. RECURSIVIDAD (Estructuras de selección en vez de ciclos explícitos)
   Uso: Soluciona problemas reduciendo su tamaño en cada llamada hasta un caso base.
   ========================================================================== */

// Caso de estudio estándar: Factorial de un número
long int factorial(int n) {
    if (n == 0 || n == 1) { // CASO BASE: Condición de parada obligatoria para evitar Stack Overflow
        return 1;           // El factorial de 0 y 1 es por definición 1
    } else {                // PASO RECURSIVO: Llama a la función con un tamaño de problema menor
        return n * factorial(n - 1); // Multiplica el número actual por el factorial de n-1
    }
}

// Caso de estudio estándar: Sucesión de Fibonacci
int fibonacci(int n) {
    if (n == 0) return 0;   // CASO BASE 1: Elemento cero
    if (n == 1) return 1;   // CASO BASE 2: Primer elemento
    return fibonacci(n - 1) + fibonacci(n - 2); // PASO RECURSIVO doble
}

#endif // Fin de las directivas del preprocesador del repositorio
