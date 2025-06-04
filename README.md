# Poyecto_estructura_de_datos
#include <iostream>
using namespace std;

struct Proceso {
    int id;
    string estado; // "listo", "ejecutando", "bloqueado"
    int prioridad;
    int memoria[100]; // pila de memoria simple (arreglo)
    int topeMemoria;
    Proceso* siguiente;
};

Proceso* cabeza = NULL;
int siguienteID = 1;

// Agregar proceso al final de la lista
void crearProceso(string estado, int prioridad) {
    Proceso* nuevo = new Proceso;
    nuevo->id = siguienteID++;
    nuevo->estado = estado;
    nuevo->prioridad = prioridad;
    nuevo->topeMemoria = -1;
    nuevo->siguiente = NULL;

    if (cabeza == NULL) {
        cabeza = nuevo;
    } else {
        Proceso* aux = cabeza;
        while (aux->siguiente != NULL) aux = aux->siguiente;
        aux->siguiente = nuevo;
    }
    cout << "Proceso creado con ID: " << nuevo->id << ", Estado: " << estado << endl;
}
void crearProceso(string estado, string prioridad) {
    if (estado == "invalido" || prioridad == "invalida") {
        cout << "Estado o prioridad inválida.\n";
        return;
    }

    Proceso* nuevo = new Proceso;
    nuevo->id = siguienteID++;
    nuevo->estado = estado;
    nuevo->prioridad = prioridad;
    nuevo->topeMemoria = -1;
    nuevo->siguiente = NULL;

    if (cabeza == NULL) {
        cabeza = nuevo;
    } else {
        Proceso* aux = cabeza;
        while (aux->siguiente != NULL)
            aux = aux->siguiente;
        aux->siguiente = nuevo;
    }

    procesosCreados++;
    cout << "Proceso creado con ID: " << nuevo->id << ", Estado: " << estado << ", Prioridad: " << prioridad << endl;

    if (procesosCreados == 4)
        cout << " Memoria casi llena (por cantidad de procesos).\n";
    else if (procesosCreados == 5)
        cout << " Memoria llena (por cantidad de procesos).\n";
}

void mostrarProcesos() {
    Proceso* aux = cabeza;
    cout << "\n--- Procesos registrados ---\n";
    while (aux != NULL) {
        cout << "ID: " << aux->id
             << ", Estado: " << aux->estado
             << ", Prioridad: " << aux->prioridad
             << ", Memoria usada (pila): " << aux->topeMemoria + 1;
        if (aux->topeMemoria + 1 >= 90)
            cout << " (¡Memoria casi llena!)";
        cout << endl;
        aux = aux->siguiente;
    }

    cout << "Memoria disponible del sistema: " << memoriaDisponible << " unidades\n";

    if (memoriaDisponible == 0)
        cout << " Memoria del sistema llena.\n";
    else if (memoriaDisponible <= 1)
        cout << " Advertencia: Memoria del sistema casi llena.\n";
}

void cambiarEstado(int id, string nuevoEstado) {
    if (nuevoEstado == "invalido") {
        cout << "Estado inválido.\n";
        return;
    }

    Proceso* aux = cabeza;
    while (aux != NULL) {
        if (aux->id == id) {
            aux->estado = nuevoEstado;
            cout << "Estado del proceso " << id << " cambiado a " << nuevoEstado << endl;
            return;
        }
        aux = aux->siguiente;
    }

    cout << "Proceso no encontrado.\n";
}
