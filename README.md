#include <iostream>
#include <string>

using namespace std;

// Definición de la estructura del nodo del árbol
struct Estudiante {
    int codigo;
    string nombres;
    string apellidos;
    string fechaNacimiento;
    Estudiante* izquierda;
    Estudiante* derecha;
    
    Estudiante(int cod, string nom, string ape, string fecha) {
        codigo = cod;
        nombres = nom;
        apellidos = ape;
        fechaNacimiento = fecha;
        izquierda = nullptr;
        derecha = nullptr;
    }
};

// Clase para el árbol binario de estudiantes
class ArbolEstudiantes {
private:
    Estudiante* raiz;

    // Métodos privados para insertar y eliminar nodos de manera recursiva
    Estudiante* insertarRecursivo(Estudiante* nodo, int codigo, string nombres, string apellidos, string fecha);
    Estudiante* eliminarRecursivo(Estudiante* nodo, int codigo);
    Estudiante* encontrarMinimo(Estudiante* nodo);
    void inordenRecursivo(Estudiante* nodo);
    void preordenRecursivo(Estudiante* nodo);
    void postordenRecursivo(Estudiante* nodo);

public:
    ArbolEstudiantes() {
        raiz = nullptr;
    }

    // Métodos públicos para insertar, eliminar y recorrer el árbol
    void insertar(int codigo, string nombres, string apellidos, string fecha);
    void eliminar(int codigo);
    void recorrerInorden();
    void recorrerPreorden();
    void recorrerPostorden();
};

// Método privado para insertar nodos de manera recursiva
Estudiante* ArbolEstudiantes::insertarRecursivo(Estudiante* nodo, int codigo, string nombres, string apellidos, string fecha) {
    if (nodo == nullptr) {
        return new Estudiante(codigo, nombres, apellidos, fecha);
    }

    // Insertar a la izquierda si el código es menor
    if (codigo < nodo->codigo) {
        nodo->izquierda = insertarRecursivo(nodo->izquierda, codigo, nombres, apellidos, fecha);
    }
    // Insertar a la derecha si el código es mayor o igual
    else {
        nodo->derecha = insertarRecursivo(nodo->derecha, codigo, nombres, apellidos, fecha);
    }

    return nodo;
}

// Método público para insertar nodos
void ArbolEstudiantes::insertar(int codigo, string nombres, string apellidos, string fecha) {
    raiz = insertarRecursivo(raiz, codigo, nombres, apellidos, fecha);
}

// Método privado para encontrar el nodo mínimo de un subárbol
Estudiante* ArbolEstudiantes::encontrarMinimo(Estudiante* nodo) {
    while (nodo->izquierda != nullptr) {
        nodo = nodo->izquierda;
    }
    return nodo;
}

// Método privado para eliminar nodos de manera recursiva
Estudiante* ArbolEstudiantes::eliminarRecursivo(Estudiante* nodo, int codigo) {
    if (nodo == nullptr) {
        return nodo;
    }

    // Buscar el nodo a eliminar
    if (codigo < nodo->codigo) {
        nodo->izquierda = eliminarRecursivo(nodo->izquierda, codigo);
    } else if (codigo > nodo->codigo) {
        nodo->derecha = eliminarRecursivo(nodo->derecha, codigo);
    } else {
        // Nodo encontrado, proceder a eliminarlo

        // Caso 1: Nodo sin hijos o con un solo hijo
        if (nodo->izquierda == nullptr) {
            Estudiante* temp = nodo->derecha;
            delete nodo;
            return temp;
        } else if (nodo->derecha == nullptr) {
            Estudiante* temp = nodo->izquierda;
            delete nodo;
            return temp;
        }

        // Caso 2: Nodo con dos hijos, encontrar el sucesor inorden
        Estudiante* sucesor = encontrarMinimo(nodo->derecha);

        // Copiar los datos del sucesor al nodo actual
        nodo->codigo = sucesor->codigo;
        nodo->nombres = sucesor->nombres;
        nodo->apellidos = sucesor->apellidos;
        nodo->fechaNacimiento = sucesor->fechaNacimiento;

        // Eliminar el sucesor
        nodo->derecha = eliminarRecursivo(nodo->derecha, sucesor->codigo);
    }

    return nodo;
}

// Método público para eliminar nodos
void ArbolEstudiantes::eliminar(int codigo) {
    raiz = eliminarRecursivo(raiz, codigo);
}

// Métodos privados para recorrer el árbol de manera recursiva
void ArbolEstudiantes::inordenRecursivo(Estudiante* nodo) {
    if (nodo == nullptr) {
        return;
    }
    inordenRecursivo(nodo->izquierda);
    cout << "Código: " << nodo->codigo << ", Nombres: " << nodo->nombres << ", Apellidos: " << nodo->apellidos << ", Fecha de Nacimiento: " << nodo->fechaNacimiento << endl;
    inordenRecursivo(nodo->derecha);
}

void ArbolEstudiantes::preordenRecursivo(Estudiante* nodo) {
    if (nodo == nullptr) {
        return;
    }
    cout << "Código: " << nodo->codigo << ", Nombres: " << nodo->nombres << ", Apellidos: " << nodo->apellidos << ", Fecha de Nacimiento: " << nodo->fechaNacimiento << endl;
    preordenRecursivo(nodo->izquierda);
    preordenRecursivo(nodo->derecha);
}

void ArbolEstudiantes::postordenRecursivo(Estudiante* nodo) {
    if (nodo == nullptr) {
        return;
    }
    postordenRecursivo(nodo->izquierda);
    postordenRecursivo(nodo->derecha);
    cout << "Código: " << nodo->codigo << ", Nombres: " << nodo->nombres << ", Apellidos: " << nodo->apellidos << ", Fecha de Nacimiento: " << nodo->fechaNacimiento << endl;
}

// Métodos públicos para recorrer el árbol
void ArbolEstudiantes::recorrerInorden() {
    inordenRecursivo(raiz);
}

void ArbolEstudiantes::recorrerPreorden() {
    preordenRecursivo(raiz);
}

void ArbolEstudiantes::recorrerPostorden() {
    postordenRecursivo(raiz);
}

int main() {
    ArbolEstudiantes arbol;

    // Insertar estudiantes
    arbol.insertar(1001, "Juan", "Perez", "2000-01-10");
    arbol.insertar(1002, "Maria", "Gomez", "1999-05-15");
    arbol.insertar(1003, "Carlos", "Lopez", "2001-11-25");

    // Recorrer el árbol
    cout << "Recorrido Inorden:" << endl;
    arbol.recorrerInorden();
    cout << endl;

    cout << "Recorrido Preorden:" << endl;
    arbol.recorrerPreorden();
    cout << endl;

    cout << "Recorrido Postorden:" << endl;
    arbol.recorrerPostorden();
    cout << endl;

    // Eliminar un estudiante
    arbol.eliminar(1002);

    // Recorrer el árbol después de eliminar un estudiante
    cout << "Recorrido Inorden después de eliminar un estudiante:" << endl;
    arbol.recorrerInorden();
    cout << endl;

    return 0;
}
