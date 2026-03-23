
Método 1: insertarFinal1. Código original

   void insertarFinal(int v) {
        Nodo* nuevo = new Nodo(v);
        if (cabeza == NULL) { 
            cabeza = nuevo; 
            return; 
        }
        
        // Cuento cuántos elementos hay en total
        int tamano = 0;
        Nodo* contador = cabeza;
        while (contador != NULL) {
            tamano++;
            contador = contador->siguiente;
        }
        
        // Recorro de nuevo desde el inicio hasta esa posición
        Nodo* temp = cabeza;
        for (int i = 1; i < tamano; i++) {
            temp = temp->siguiente;
        }
        temp->siguiente = nuevo;
    }


 -- Preguntas de la IA:
    Veo que en tu código primero cuentas cuántos nodos hay usando un while, y luego usas un ciclo for para llegar a esa posición.
    ¿Por qué decidiste recorrer la lista dos veces?
        Yo: Porque quería saber el tamaño exacto para saber en qué "índice" detenerme, como lo haría normalmente al trabajar con un arreglo.
   IA: Entiendo la lógica, pero recuerda que en una lista enlazada no tenemos índices fijos. Si tuvieras 1,000 nodos, tu programa daría 1,000 pasos para contarlos y otros 999 pasos para llegar al final.
  ¿Habría alguna forma de llegar al último nodo dando un solo recorrido?
        Yo: Tienes razón, es muy ineficiente. Podría simplemente avanzar nodo por nodo usando un solo while que se detenga cuando el puntero siguiente apunte a NULL, sin necesidad de contar nada.

  -- Bugs o mejoras descubiertas:Descubrí que estaba arrastrando mi mentalidad de "arreglos" al trabajar con listas enlazadas. Al intentar simular un índice, estaba haciendo que mi algoritmo recorriera la lista dos veces,
     lo que afectaba el rendimiento. La mejora consistió en eliminar el contador y el ciclo for, sustituyéndolos por una sola iteración directa hasta encontrar el último nodo, optimizando la complejidad a $O(n)$.

 Codigo corregido:
    void insertarFinal(int v) {
        Nodo* nuevo = new Nodo(v);
        if (cabeza == NULL) { 
            cabeza = nuevo; 
            return; 
        }
        
        Nodo* temp = cabeza;
        while (temp->siguiente != NULL) {
            temp = temp->siguiente;
        }
        temp->siguiente = nuevo;
    }

---


Método 2: eliminarInicio
void eliminarInicio() {
        if (cabeza == NULL) return;
        
        // Muevo la cabeza un espacio hacia adelante para olvidar el primero
        cabeza = cabeza->siguiente; 
    }

-- Preguntas de la IA y mis respuestas:
IA: Tu código dice cabeza = cabeza->siguiente;. Efectivamente, esto hace que la lista ahora empiece desde el segundo nodo. Pero, ¿qué pasó con el espacio en memoria del primer nodo que acabas de saltarte? 
    Yo: Supongo que se queda ahí, pero ya no forma parte de la lista porque nada lo está apuntando y no puedo acceder a él.

IA: ¡Exacto! Queda flotando. En C++, ¿qué pasa si creamos elementos en la memoria dinámica usando new pero nunca los destruimos explícitamente cuando dejamos de usarlos?
    Yo: Se genera una fuga de memoria. Si este método se llamara repetidamente a lo largo del programa, terminaría consumiendo toda la memoria RAM disponible del sistema.


-- Bugs o mejoras descubiertas:
Me di cuenta de que simplemente mover el puntero de la cabeza no es suficiente en C++. Descubrí la importancia del manejo manual de memoria:
es estrictamente necesario guardar de forma temporal la dirección del nodo que vamos a desconectar de la lista, para luego poder aplicarle la instrucción delete y devolver esa memoria al sistema operativo.


Codigo corregido:
void eliminarInicio() {
        if (cabeza == NULL) return;
        
        Nodo* aBorrar = cabeza;
        cabeza = cabeza->siguiente;
        delete aBorrar;
    }


---


Método 3: buscar
bool buscar(int v) {
        if (cabeza == NULL) return false;
        
        Nodo* temp = cabeza;
        // Busco mientras no llegue al final
        while (temp->siguiente != NULL) {
            if (temp->dato == v) {
                return true;
            }
            temp = temp->siguiente;
        }
        return false;
    }


-- Preguntas de la IA y mis respuestas:
IA: Miremos la condición de tu bucle: while (temp->siguiente != NULL). Al ejecutar tu programa de prueba, me comentaste que no detectaba el número 20 (que era el último de la lista). ¿Qué pasa cuando temp llega exactamente a ese último nodo?
     Yo: Como es el último nodo, su puntero siguiente es NULL.

IA: Correcto. Y si su siguiente es NULL, ¿entrará el programa al ciclo while para revisar si el dato coincide con el que buscas?
     Yo: La condición del while se evalúa como falsa antes de entrar a revisar el dato, por lo que el ciclo se rompe y la función retorna false, ignorando por completo el último elemento.

IA: ¡Ahí está el bug! ¿Cómo podrías cambiar la condición para asegurarte de revisar todos los nodos sin saltarte el último?
     Yo: Debería cambiar la condición a while (temp != NULL). De esta forma, el ciclo entra, evalúa el dato actual, y hasta después intenta avanzar al siguiente, cubriendo toda la lista.

Codigo corregido:
bool buscar(int v) {
        Nodo* temp = cabeza;
        while (temp != NULL) {
            if (temp->dato == v) return true;
            temp = temp->siguiente;
        }
        return false;
    }

    
---



