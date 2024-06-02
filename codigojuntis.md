#include<iostream>
#include<cstdlib>
using namespace std;

struct nodo{
    int nro;
    struct nodo *sgte;
};

typedef struct nodo *Puntero;

class Pila{
    public:
        Pila(void);
        void Apilar(int );
        int Desapilar(void );
        void Cima(void);
        bool PilaVacia(void);
        void MostrarPila(void);
        void DestruirPila(void);

    private:
        Puntero cima;

};
class Cola{
    public:
        Cola(void);
        void Encolar(int );
        int Desencolar(void );
        bool ColaVacia(void);
        void MostrarCola(void);
        void VaciarCola(void);

    private:
        Puntero delante;
        Puntero atras;

};
Pila::Pila(void){
    cima=NULL;
}

bool Pila::PilaVacia(void){
    if(cima==NULL)
        return true;
    else
        return false;
}

void Pila::Apilar(int x){

    Puntero p_aux;
    p_aux=new(struct nodo);
    p_aux->nro=x;
    p_aux->sgte=cima;
    cima=p_aux;

}

void Pila::Cima(){
    int x;
    if(cima==NULL)
        cout<<"\n\n\tPila Vacia...!";

    else {
        x=cima->nro;
        cout<<"\n\tLa Cima es :"<<x<<endl;
    }
}

int Pila::Desapilar(void){
    int x;
    Puntero p_aux;
    if(cima==NULL)
        cout<<"\n\n\tPila Vacia...!!";
    else{
        p_aux=cima;
        x=p_aux->nro;
        cima=cima->sgte;
        delete(p_aux);
    }
    return x;
}

void Pila::MostrarPila(void){
    Puntero p_aux;
    p_aux=cima;

    while(p_aux!=NULL){
        cout<<"\t "<<p_aux->nro<<endl;
        p_aux=p_aux->sgte;
    }
}

void Pila::DestruirPila(void){
    Puntero p_aux;

    while(cima!=NULL){
            p_aux=cima;
            cima=cima->sgte;
            delete(p_aux);
    }
}
Cola::Cola(void){
    delante=NULL;//inicializamos los punteros
    atras=NULL;
}

bool Cola::ColaVacia(){

    if(delante==NULL)
        return true;
    else return false;
}
void Cola::Encolar(int x){

     Puntero p_aux;

     p_aux=new(struct nodo);
     p_aux->nro=x;
     p_aux->sgte=NULL;

     if(delante==NULL)
        delante=p_aux;
     else atras->sgte=p_aux;

     atras=p_aux;
}

int Cola::Desencolar(void){

    int n;
    Puntero p_aux;

    p_aux=delante;
    n=p_aux->nro;
    delante=(delante)->sgte;
    delete(p_aux);
    return n;
}

void Cola::MostrarCola(void){
    Puntero p_aux;
     p_aux=delante;
     while(p_aux!=NULL){
        cout<<"\n\n\t"<<p_aux->nro;
        p_aux=p_aux->sgte;
     }

}

void Cola::VaciarCola(void){

     Puntero p_aux,r_aux;
     p_aux=delante;
     while(p_aux!=NULL){
        r_aux=p_aux;
        p_aux=p_aux->sgte;
        delete(r_aux);
     }
     delante=NULL;
     atras=NULL;
}

void menu(void)
{
    cout<<"\t -------------------------------------------\n";
    cout<<"\t|        IMPLEMENTACION DE UNA PILA         |\n";
    cout<<"\t|-------------------------------------------|\n";
    cout<<" \t|                                           |"<<endl;
    cout<<" \t|  1. APILAR                                |"<<endl;
    cout<<" \t|  2. DESAPILAR                             |"<<endl;
    cout<<" \t|  3. MOSTRAR PILA                          |"<<endl;
    cout<<" \t|  4. DESTRUIR PILA                         |"<<endl;
    cout<<" \t|  5. MOSTRAR CIMA                          |"<<endl;
    cout<<" \t|  6. ENCOLAR                               |"<<endl;
    cout<<" \t|  7. DESENCOLAR                            |"<<endl;
    cout<<" \t|  8. MOSTRAR COLA                          |"<<endl;
    cout<<" \t|  9. VACIAR COLA                           |"<<endl;
    cout<<" \t|  10. SALIR                                |"<<endl;
    cout<<"\t -------------------------------------------\n";
    cout<<"\t Ingrese opcion: ";
}

int main(void ){

    system("color 020");
    Pila pila;
    Cola cola;
    int x;
    int opc;

    do
    {
        menu();  cin>> opc;

        switch(opc)
        {
            case 1: cout<< "\n\t INGRESE NUMERO A APILAR: "; cin>> x;
                    pila.Apilar(x);
                    cout<<"\n\n\t\tNumero " << x << " apilado...\n\n";
                    break;

            case 2:
                    if(pila.PilaVacia()==true)
                        cout<<"\n\n\tPila Vacia....";
                    else{
                        x = pila.Desapilar( );
                        cout<<"\n\n\tNumero "<<x<<" desapilado\n";
                        }
                    break;


            case 3:
                    cout << "\n\n\t MOSTRANDO PILA\n\n";
                    if(pila.PilaVacia()!=true)
                        pila.MostrarPila(  );
                    else
                        cout<<"\n\n\tPila vacia..!"<<endl;
                    break;


            case 4:
                    pila.DestruirPila(  );
                    cout<<"\n\n\t\tPila eliminada...\n\n";
                    break;
            case 5:
                    pila.Cima();
                    break;
                    
            case 6:
			        cout<< "\n\t INGRESE NUMERO A ENCOLAR: "; cin>> x;
                    cola.Encolar(x);
                    cout<<"\n\n\t\tNumero " << x << " ENCOLADO...\n\n";
                    break;

            case 7:
                    if(cola.ColaVacia()==true)
                        cout<<"\n\n\tCola Vacia....";
                    else{
                        x = cola.Desencolar( );
                        cout<<"\n\n\tNumero "<<x<<" Desencolado\n";
                        }
                    break;


            case 8:
                    cout << "\n\n\t MOSTRANDO COLA\n\n";
                    if(cola.ColaVacia()!=true)
                        cola.MostrarCola(  );
                    else
                        cout<<"\n\n\tCola vacia..!"<<endl;
                    break;


            case 9:
                    cola.VaciarCola(  );
                    cout<<"\n\n\t\tCola eliminada...\n\n";
                    break;

         }

        cout<<endl<<endl;
        system("pause");
        system("cls");

    }while(opc!=10);

return 0;
}
//lista
#include<iostream>
#include<cstdlib>

using namespace std;

struct nodo{
    int dato;
    nodo* siguiente;
}*primero, *ultimo;

void insertarNodo();
void buscarNodo();
void modificarNodo();
void mostrarLista();
void eliminarNodo();
void ordenarBurbuja();
void ordenarInsercion();
void menu();

int main()
{
    system("color 0F");
    int op;
    
    do{
        menu();
        cin>>op;
        switch(op){
            case 1:
                cout<<"\n\nINSERTAR DATOS EN LA LISTA \n\n";
                insertarNodo();
                break;
            case 2:
                cout<<"\n\nBUSCAR DATOS EN LA LISTA \n\n";
                buscarNodo();
                break;
            case 3:
                cout<<"\n\nMODIFICAR DATOS EN LA LISTA \n\n";
                modificarNodo();
                break;
            case 4:
                cout<<"\n\nELIMINAR DATOS EN LA LISTA \n\n";
                eliminarNodo();
                break;
            case 5:
                cout<<"\n\nMOSTRAR DATOS EN LA LISTA \n\n";
                mostrarLista();
                break;
            case 6:
                cout<<"\n\nORDENAR LA LISTA POR BURBUJA \n\n";
                ordenarBurbuja();
                break;
            case 7:
                cout<<"\n\nORDENAR LA LISTA POR INSERCIÓN \n\n";
                ordenarInsercion();
                break;
            case 8:
                cout<<"\n\nSALIR DEL PROGRAMA .... \n\n";
                break;
            default:
                cout<<"\n\nOpcion no valida ... \n\n";
        }
        cout<<endl<<endl;
        system("pause");
        system("cls");
    }while(op!=8);
    
    return 0;
}

void insertarNodo(){
    nodo* nuevo = new nodo();
    cout<<"Inserte un elemento a enlistar: ";
    cin>>nuevo->dato;
    
    if(primero==NULL){
        primero = nuevo;
        primero->siguiente=NULL;
        ultimo=nuevo;
    } else{
        ultimo->siguiente=nuevo;
        nuevo->siguiente=NULL;
        ultimo=nuevo;
    }
    cout<<"Nodo ingresado\n\n";
}

void buscarNodo(){
    nodo* actual = new nodo();
    actual=primero;
    bool encontrado=false;
    int nodoBuscar=0;
    
    cout<<"\n Ingrese el dato a buscar en la Lista: ";
    cin>>nodoBuscar;
    
    if(primero != NULL){
        while(actual != NULL && encontrado!=true){
            if(actual->dato==nodoBuscar){
                cout<<"\n Elemento "<<nodoBuscar<<" existente \n\n";
                encontrado=true;
            }
            actual=actual->siguiente;
        }
        if(!encontrado){
            cout<<"\n Elemento no encontrado...!!!";
        }
    }else{
        cout<<"La lista se encuentra vacia...!!";
    }
}

void mostrarLista(){
    nodo* actual = new nodo();
    actual=primero;
    if(primero!=NULL){
        while(actual!=NULL){
            cout<<" "<<actual->dato<<endl;
            actual=actual->siguiente;
        }
    }else{
        cout<<"La lista se encuentra vacia...!!";
    }
}

void modificarNodo(){
    nodo* actual = new nodo();
    actual=primero;
    bool encontrado=false;
    int nodoBuscar=0;
    
    cout<<"\n Ingrese el dato a buscar en la Lista: ";
    cin>>nodoBuscar;
    
    if(primero != NULL){
        while(actual != NULL && encontrado!=true){
            if(actual->dato==nodoBuscar){
                cout<<"\n Elemento "<<nodoBuscar<<" existente";
                
                cout<<"\n\n Inserte el nuevo valor: ";
                cin>>actual->dato;
                cout<<"\n\tEl dato anterior: "<<nodoBuscar<<" se modifico con exito por: "<<actual->dato;
                encontrado=true;
            }
            actual=actual->siguiente;
        }
        if(!encontrado){
            cout<<"\n Elemento no encontrado...!!!";
        }
    }else{
        cout<<"La lista se encuentra vacia...!!";
    }
}

void eliminarNodo(){
    nodo* actual = new nodo();
    actual=primero;
    nodo* anterior=new nodo();
    anterior=NULL;
    bool encontrado=false;
    int nodoBuscar=0;
    
    cout<<"\n Ingrese el dato a buscar Para eliminarlo: ";
    cin>>nodoBuscar;
    
    if(primero != NULL){
        while(actual != NULL && encontrado!=true){
            if(actual->dato==nodoBuscar){
                cout<<"\n Elemento "<<nodoBuscar<<" existente \n\n";
                
                if(actual==primero){ //eliminar el primer elemento de la lista
                    primero=primero->siguiente;
                }else if(actual==ultimo){ //Eliminar el ultimo elemento de la lista
                    anterior->siguiente=NULL;
                    ultimo=anterior;
                }else{ //Eliminar cualquier elemento
                    anterior->siguiente=actual->siguiente;
                }
                cout<<"\n Nodo " <<nodoBuscar<<" Eliminado ";
                encontrado=true;
            }
            anterior=actual;
            actual=actual->siguiente;
        }
        if(!encontrado){
            cout<<"\n Elemento no encontrado...!!!";
        }
    }else{
        cout<<"La lista se encuentra vacia...!!";
    }
}

void ordenarBurbuja(){
    nodo* actual = new nodo();
    actual = primero;
    int temp;
    if(primero != NULL){
        while(actual->siguiente != NULL){
            nodo* siguiente = new nodo();
            siguiente = actual->siguiente;
            while(siguiente != NULL){
                if(actual->dato > siguiente->dato){
                    temp = actual->dato;
                    actual->dato = siguiente->dato;
                    siguiente->dato = temp;
                }
                siguiente = siguiente->siguiente;
            }
            actual = actual->siguiente;
        }
        cout<<"\n\nLa lista ha sido ordenada por el método de Burbuja\n\n";
    }else{
        cout<<"La lista se encuentra vacia...!!";
    }
}

void ordenarInsercion(){
    nodo* actual = new nodo();
    actual = primero;
    if(primero != NULL){
        while(actual->siguiente != NULL){
            nodo* siguiente = new nodo();
            siguiente = actual->siguiente;
            while(siguiente != NULL){
                if(actual->dato > siguiente->dato){
                    int temp = actual->dato;
                    actual->dato = siguiente->dato;
                    siguiente->dato = temp;
                }
                siguiente = siguiente->siguiente;
            }
            actual = actual->siguiente;
        }
        cout<<"\n\nLa lista ha sido ordenada por el método de Inserción\n\n";
    }else{
        cout<<"La lista se encuentra vacía...!!";
    }
}

void menu(){
    cout<<" \t|-------------------------------------------|\n";
    cout<<" \t|     IMPLEMENTACION DE UNA LISTA SIMPLE    |\n";
    cout<<" \t|-------------------------------------------|\n";
    cout<<" \t|                                         |\n";
    cout<<" \t| 1. INSERTAR                              |\n";
    cout<<" \t| 2. BUSCAR                                |\n";
    cout<<" \t| 3. MODIFICAR                             |\n";
    cout<<" \t| 4. ELIMINAR                              |\n";
    cout<<" \t| 5. MOSTRAR                               |\n";
    cout<<" \t| 6. ORDENAR POR BURBUJA                   |\n";
    cout<<" \t| 7. ORDENAR POR INSERCION                 |\n";
    cout<<" \t| 8. SALIR                                 |\n";
    cout<<" \t|                                         |\n";
    cout<<" \t|-------------------------------------------|\n";
    cout<<"\n\t Ingrese opcion: ";
}
//ordenamiento
#include <iostream>

using namespace std;

// Nodo de la lista
struct Nodo {
    int dato;
    Nodo* siguiente;
};

// Clase para la lista simple
class ListaSimple {
private:
    Nodo* cabeza;

public:
    ListaSimple() : cabeza(NULL) {}

    // Método para insertar un elemento al inicio de la lista
    void insertar(int dato) {
        Nodo* nuevoNodo = new Nodo();
        nuevoNodo->dato = dato;
        nuevoNodo->siguiente = cabeza;
        cabeza = nuevoNodo;
    }

    // Método para imprimir la lista
    void imprimir() {
        Nodo* actual = cabeza;
        cout << "Lista: ";
        while (actual != NULL) {
            cout << actual->dato << " ";
            actual = actual->siguiente;
        }
        cout << endl;
    }

    // Método para intercambiar dos elementos de la lista
    void intercambiar(Nodo* a, Nodo* b) {
        int temp = a->dato;
        a->dato = b->dato;
        b->dato = temp;
    }

    // Método para ordenar la lista por el método de selección
    void ordenarSeleccion() {
        Nodo* actual = cabeza;
        while (actual != NULL) {
            Nodo* min = actual;
            Nodo* siguiente = actual->siguiente;
            while (siguiente != NULL) {
                if (siguiente->dato < min->dato) {
                    min = siguiente;
                }
                siguiente = siguiente->siguiente;
            }
            intercambiar(actual, min);
            actual = actual->siguiente;
        }
    }

    // Métodos para QuickSort
    Nodo* obtenerUltimo(Nodo* nodo) {
        while (nodo != NULL && nodo->siguiente != NULL) {
            nodo = nodo->siguiente;
        }
        return nodo;
    }

    Nodo* particion(Nodo* inicio, Nodo* fin, Nodo** nuevoInicio, Nodo** nuevoFin) {
        Nodo* pivote = fin;
        Nodo* anterior = NULL;
        Nodo* cur = inicio;
        Nodo* tail = pivote;

        while (cur != pivote) {
            if (cur->dato < pivote->dato) {
                if ((*nuevoInicio) == NULL) {
                    (*nuevoInicio) = cur;
                }
                anterior = cur;
                cur = cur->siguiente;
            } else {
                if (anterior != NULL) {
                    anterior->siguiente = cur->siguiente;
                }
                Nodo* temp = cur->siguiente;
                cur->siguiente = NULL;
                tail->siguiente = cur;
                tail = cur;
                cur = temp;
            }
        }
        if ((*nuevoInicio) == NULL) {
            (*nuevoInicio) = pivote;
        }
        (*nuevoFin) = tail;
        return pivote;
    }

    Nodo* quickSort(Nodo* inicio, Nodo* fin) {
        if (!inicio || inicio == fin) {
            return inicio;
        }

        Nodo* nuevoInicio = NULL;
        Nodo* nuevoFin = NULL;
        Nodo* pivote = particion(inicio, fin, &nuevoInicio, &nuevoFin);

        if (nuevoInicio != pivote) {
            Nodo* temp = nuevoInicio;
            while (temp->siguiente != pivote) {
                temp = temp->siguiente;
            }
            temp->siguiente = NULL;

            nuevoInicio = quickSort(nuevoInicio, temp);

            temp = obtenerUltimo(nuevoInicio);
            temp->siguiente = pivote;
        }

        pivote->siguiente = quickSort(pivote->siguiente, nuevoFin);

        return nuevoInicio;
    }

    // Método para ordenar la lista por el método QuickSort
    void ordenarQuickSort() {
        cabeza = quickSort(cabeza, obtenerUltimo(cabeza));
    }
};

int main() {
    ListaSimple lista;

    int opcion;
    do {
        cout << "\n¿Que caso elige?\n";
        cout << "1. Insertar un elemento\n";
        cout << "2. Ordenar la lista por seleccion\n";
        cout << "3. Ordenar la lista por QuickSort\n";
        cout << "4. Imprimir la lista original\n";
        cout << "5. Salir\n";
        cout << "Opcion: ";
        cin >> opcion;

        switch (opcion) {
            case 1: {
                int elemento;
                cout << "Ingrese el elemento a insertar: ";
                cin >> elemento;
                lista.insertar(elemento);
                break;
            }
            case 2:
                lista.ordenarSeleccion();
                cout << "Lista ordenada por seleccion:\n";
                lista.imprimir();
                break;
            case 3:
                lista.ordenarQuickSort();
                cout << "Lista ordenada por QuickSort:\n";
                lista.imprimir();
                break;
            case 4:
                cout << "Lista original:\n";
                lista.imprimir();
                break;
            case 5:
                cout << "Saliendo del programa.\n";
                break;
            default:
                cout << "Opción no válida. Por favor, elija nuevamente.\n";
                break;
        }
    } while (opcion != 5);

    return 0;
}
//ordenamientos
#include <iostream>
#include <vector>
#include <algorithm>

#ifdef _WIN32
    #include <windows.h>
#else
    #include <unistd.h>
#endif

using namespace std;

// Función para limpiar la pantalla
void limpiarPantalla() {
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
}

// Imprimir vector
void imprimirVector(const vector<int> &vec) {
    for (size_t i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
}

// Método de inserción
void ordenarPorInsercion(vector<int> &vec) {
    int n = vec.size();
    for (int i = 1; i < n; ++i) {
        int key = vec[i];
        int j = i - 1;
        while (j >= 0 && vec[j] > key) {
            vec[j + 1] = vec[j];
            j = j - 1;
        }
        vec[j + 1] = key;
    }
    cout << "Vector ordenado por metodo de insercion: ";
    imprimirVector(vec);
}

// Método de selección
void ordenarPorSeleccion(vector<int> &vec) {
    int n = vec.size();
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            if (vec[j] < vec[min_idx]) {
                min_idx = j;
            }
        }
        swap(vec[min_idx], vec[i]);
    }
    cout << "Vector ordenado por método de seleccion: ";
    imprimirVector(vec);
}

// Método de intercambio directo
void ordenarPorIntercambioDirecto(vector<int> &vec) {
    int n = vec.size();
    for (int i = 0; i < n - 1; ++i) {
        for (int j = i + 1; j < n; ++j) {
            if (vec[i] > vec[j]) {
                swap(vec[i], vec[j]);
            }
        }
    }
    cout << "Vector ordenado por metodo de intercambio directo: ";
    imprimirVector(vec);
}

// Quicksort
void quicksort(vector<int> &vec, int low, int high) {
    if (low < high) {
        int pivot = vec[high];
        int i = (low - 1);
        for (int j = low; j <= high - 1; ++j) {
            if (vec[j] <= pivot) {
                ++i;
                swap(vec[i], vec[j]);
            }
        }
        swap(vec[i + 1], vec[high]);
        int pi = i + 1;
        quicksort(vec, low, pi - 1);
        quicksort(vec, pi + 1, high);
    }
}

void ordenarPorQuicksort(vector<int> &vec) {
    quicksort(vec, 0, vec.size() - 1);
    cout << "Vector ordenado por metodo quicksort: ";
    imprimirVector(vec);
}

// Búsqueda de elementos
bool buscarElemento(const vector<int> &vec, int elemento) {
    char opcion;
    cout << "Elija el metodo de búsqueda (B para Binario, S para Secuencial): ";
    cin >> opcion;

    switch (opcion) {
        case 'B':
        case 'b':
            if (binary_search(vec.begin(), vec.end(), elemento)) {
                cout << "El elemento " << elemento << " esta en el vector." << endl;
                return true;
            } else {
                cout << "El elemento " << elemento << " no esta en el vector." << endl;
                return false;
            }
            break;
        case 'S':
        case 's':
            for (size_t i = 0; i < vec.size(); ++i) {
                if (vec[i] == elemento) {
                    cout << "El elemento " << elemento << " esta en el vector." << endl;
                    return true;
                }
            }
            cout << "El elemento " << elemento << " no esta en el vector." << endl;
            return false;
            break;
        default:
            cout << "Opción no valida." << endl;
            return false;
    }
}

int main() {
    vector<int> numeros;

    while (true) {
        limpiarPantalla();
        int opcion;
        cout << "\n***********\n";
        cout << "*       MENU DE OPCIONES    *\n";
        cout << "***********\n";
        cout << "1. Insertar numero\n";
        cout << "2. Ordenar vector\n";
        cout << "3. Buscar elemento\n";
        cout << "4. Salir\n";
        cout << "seleccione  una opcion: ";
        cin >> opcion;

        switch (opcion) {
            case 1: {
                int num;
                cout << "Ingrese un numero entero: ";
                cin >> num;
                numeros.push_back(num);
                cout << "Numero insertado." << endl;
                break;
            }
            case 2:
                if (!numeros.empty()) {
                    int metodoOrdenamiento;
                    cout << "\nMetodos de ordenamiento:\n";
                    cout << "1. Insercion\n";
                    cout << "2. Seleccion\n";
                    cout << "3. Intercambio Directo\n";
                    cout << "4. Quicksort\n";
                    cout << "Elija una opcion: ";
                    cin >> metodoOrdenamiento;

                    switch (metodoOrdenamiento) {
                        case 1:
                            ordenarPorInsercion(numeros);
                            break;
                        case 2:
                            ordenarPorSeleccion(numeros);
                            break;
                        case 3:
                            ordenarPorIntercambioDirecto(numeros);
                            break;
                        case 4:
                            ordenarPorQuicksort(numeros);
                            break;
                        default:
                            cout << "Opcion no valida." << endl;
                    }
                } else {
                    cout << "El vector esta vacio. Inserte elementos primero." << endl;
                }
                break;
            case 3:
                if (!numeros.empty()) {
                    int elemento;
                    cout << "Ingrese el elemento que desea buscar: ";
                    cin >> elemento;
                    buscarElemento(numeros, elemento);
                } else {
                    cout << "El vector esta vacio. Inserte elementos primero." << endl;
                }
                break;
            case 4:
                cout << "Saliendo del programa.";
                return 0;
            default:
                cout << "Opcion no valida." << endl;
        }
        cout << "Presione Enter para continuar...";
        cin.ignore();
        cin.get();
    }

    return 0;
}
