
1.- Diagrama de la nueva estructura

              CASO A: Lista Vacía (Todo apunta a la nada)
      cabeza -> NULL
      cola   -> NULL

             CASO B: Lista con 1 solo elemento (¡El inicio es también el final!)
      cabeza -> [ Dato: 5 | Sig: NULL ] <- cola

             CASO C: Lista normal con varios elementos
      cabeza -> [ 5 | * ] -> [ 10 | * ] -> [ 20 | NULL ] <- cola
                                       ^
        (Para insertar al final, 'cola' nos conecta directo aquí)


---

2. Código Modificado
3. 

#include <iostream>
using namespace std;

struct Nodo {
    int dato;
    Nodo* siguiente;
    Nodo(int v) {
        dato = v;
        siguiente = NULL;
    }
};

class ListaMejorada {
private:
    Nodo* cabeza;
    Nodo* cola; // puntero directo al final
    int _tamano;

public:
    ListaMejorada() {
        cabeza = NULL;
        cola = NULL; //no hay cola
        _tamano = 0;
    }

    void insertarInicio(int v) {
        Nodo* nuevo = new Nodo(v);
        nuevo->siguiente = cabeza;
        cabeza = nuevo;
        
        if (cola == NULL) {
            cola = nuevo;
        }
        _tamano++;
    }

    void insertarFinal(int v) {
        Nodo* nuevo = new Nodo(v);
        
        if (cabeza == NULL) { 
            cabeza = nuevo; 
            cola = nuevo;
        } else {
            cola->siguiente = nuevo; 
            cola = nuevo; 
        }
        _tamano++;
    }

    void eliminarInicio() {
        if (cabeza == NULL) return;
        
        Nodo* aBorrar = cabeza;
        cabeza = cabeza->siguiente;
        delete aBorrar;
        _tamano--;
        
       
        if (cabeza == NULL) {
            cola = NULL;
        }
    }

    void imprimir() {
        Nodo* temp = cabeza;
        cout << "Lista: ";
        while (temp != NULL) {
            cout << temp->dato << " -> ";
            temp = temp->siguiente;
        }
        cout << "NULL" << endl;
    }
};


---
3.- Analisis de cambio de complejidad.

insertarFinal: Pasó de $O(n)$ a $O(1)$.Antes: Si tenía 1,000 elementos, tenía que dar 1,000 saltos (temp = temp->siguiente) para llegar al último y poder enganchar el nuevo.
Ahora: Como cola ya sabe dónde está el último vagón, hacer cola->siguiente = nuevo toma solo 1 paso, sin importar si la lista tiene 10 o un millón de elementos. Es tiempo constante.

---

4.- Pruebas que demuestran la mejora.

int main() {
    ListaMejorada miLista;
    
    // Prueba de mejora O(1)
    miLista.insertarFinal(100);
    miLista.insertarFinal(200);
    miLista.insertarFinal(300);
    
    cout << "Después de tres insertarFinal rapidísimos:" << endl;
    miLista.imprimir(); //  100 -> 200 -> 300 -> NULL
    
    // Prueba del caso que me causaba bugs (vaciar la lista por completo)
    miLista.eliminarInicio();
    miLista.eliminarInicio();
    miLista.eliminarInicio();
    
    cout << "Después de vaciarla por completo:" << endl;
    miLista.imprimir(); // aqui deberia salir un ListaÑ Null

    miLista.insertarFinal(999);
    cout << "Prueba de fuego (insertar tras vaciar):" << endl;
    miLista.imprimir(); //Deberia dar un 999
    
    return 0;
}













