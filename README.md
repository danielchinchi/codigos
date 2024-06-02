#include<iostream>
#include <cstdlib>
#include <iomanip>
#include <limits>
#include <ctime>
#include <stack>
#include <vector>
#include <limits>  
#include <sstream>
#include <chrono>
#include <math.h>
//#include <codecvt>
#include <string.h>
#include <set>
#include <stdlib.h>
#include <windows.h>
#include <algorithm>

using namespace std;
void menuprin();
void menustock();
void menuprod();
void lisordenada();
///////////////////////PRODUCTOS/////////////////////////
struct Producto {
    int id;
    string nombre;
    int cantidad;
    float precio;
    string fechaCaducidad;
    Producto* siguiente;
};

Producto* primero = NULL;
Producto* ultimo = NULL;
int idActual = 1;  

struct NodoPila {
    Producto* producto;
    NodoPila* siguiente;
};
NodoPila* c = NULL;
NodoPila* e = NULL;

struct NodoArbol {
    string ciudad;
    NodoArbol* izquierda;
    NodoArbol* derecha;

    NodoArbol(string nombre) : ciudad(nombre), izquierda(NULL), derecha(NULL) {}
};
NodoArbol* buscarCiudad(NodoArbol* raiz, const string& ciudad);
bool vender(NodoArbol* arbolCiudades);
void mostrarRuta(NodoArbol* arbolCiudades, const string& ciudadEncontrada);
bool obtenerRuta(NodoArbol* raiz, const string& destino, vector<string>& ruta);

void insertarProducto(const string& nombre = "", int cantidad = 0, float precio = 0.0, const string& fechaCaducidad = "");
void ProductosIniciales();
void buscarProducto(); 
void modificarProducto();
void eliminarProducto();
void mostrarStock(); //colas
void orPrecio();
void orCaducidad(Producto* A[], int primero, int ultimo);
void orCantidad(); //ordenamiento por insercion
void mostrarLista();
void mostrarOrdenCaducidad();

Producto* binariaId(int idBuscar);
void ordradixsort(Producto *&inicio);
// pila
void pilamenu(void);
void vencimiento(void);
void pilavencidos(void);
//menus


///////////////////////CAJEROS/////////////////////////
struct SueldoInfo {
    int sueldo;
};

struct Cajero {
    int id;
    string nombre;
    string telefono;
    string sexo;
    Cajero* siguiente;
    Cajero* anterior;
    SueldoInfo sueldoInfo;  
    float antiguedad;
};

struct nodo {
    Cajero *cajero;
    struct nodo *sgte;
};

struct cola {
    nodo *delante;
    nodo *atras;
};
struct node {
    char Names[256];
    node* next;
};

const int tablesize = 10;
node* HashTable[tablesize];

struct cola colaCajeros; 
void ordenarPrioridad(struct cola &q);
void calcularSueldos(struct cola &q, struct cola &colaSueldos);
void mostrarSueldos(struct cola &colaSueldos);
bool sueldosCalculados = false;

Cajero* primeroCaj = NULL;
Cajero* ultimoCaj = NULL;
int contadorId = 0; 


void ordenarPrioridad(struct cola &q);
void calcularSueldos(struct cola &q, struct cola &colaSueldos);
void mostrarSueldos(struct cola &colaSueldos);
void limpiarColaSueldos(struct cola &colaSueldos);

int stringtokey(const char* str);
int buscarNodo(char nName[]);
int aleid();

void agregarCajero();
void buscarCajeroID();
void buscarCajeroNombre();
void modificarCajero();
void eliminarCajero();
void mostrarCajeros();
void verSueldo(Cajero* cajero);
void menuCajero();
int main(){
	system("color 5f");
	menuprin(); 
}
void lisordenada(){
	//system("cls");
    ordradixsort(primero);

    Producto* actual = primero;
		cout << "\t--------------------------------------------------------------------\n";
		cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad |\n";
		cout << "\t--------------------------------------------------------------------\n";
    while (actual != NULL) {
        
		cout << "\t| " << setw(2) << right << actual->id << " | "
		     << setw(18) << left << actual->nombre << " | "
		     << setw(8) << right << actual->cantidad << " | "
		     << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
		     << setw(18) << right << actual->fechaCaducidad << " |\n";

        actual = actual->siguiente;
    }
    cout << "\t-------------------------------------------------------------------\n";
}

//INSERTAR PRODUCTO LISTRA
void insertarProducto(const string& nombre, int cantidad, float precio, const string& fechaCaducidad) {
    Producto* nuevo = new Producto();
    nuevo->id = idActual++;
	
    if (nombre == "") {
        cout << "\tIngrese el nombre del producto: ";
        cin >> nuevo->nombre;
    } else {
        nuevo->nombre = nombre;
    }
    //mayusculas
	transform(nuevo->nombre.begin(), nuevo->nombre.end(), nuevo->nombre.begin(), ::toupper);

    if (cantidad == 0) {
        cout << "\tIngrese la cantidad: ";
        cin >> nuevo->cantidad;
    } else {
        nuevo->cantidad = cantidad;
    }

    if (precio == 0.0) {
        cout << "\tIngrese el precio: ";
        cin >> nuevo->precio;
    } else {
        nuevo->precio = precio;
    }

    if (fechaCaducidad == "") {
        cout << "\tIngrese la fecha de caducidad (formato: AAAA-MM-DD): ";
        cin >> nuevo->fechaCaducidad;
    } else {
        nuevo->fechaCaducidad = fechaCaducidad;
    }

    nuevo->siguiente = NULL;

    if (primero == NULL || nuevo->fechaCaducidad <= primero->fechaCaducidad) {
        nuevo->siguiente = primero;
        primero = nuevo;
    } else {
        Producto* actual = primero;
        while (actual->siguiente != NULL && nuevo->fechaCaducidad > actual->siguiente->fechaCaducidad) {
            actual = actual->siguiente;
        }
        nuevo->siguiente = actual->siguiente;
        actual->siguiente = nuevo;
    }    
}

//PRODUCTOS DEFINUIDOS
void ProductosIniciales(){
insertarProducto("Leche", 10, 2.5, "2022-12-01");  
insertarProducto("Pan", 20, 5, "2024-05-07");  
insertarProducto("Arroz", 5, 30,"2024-09-20");
insertarProducto("Aceite", 8, 49, "2022-03-10");
insertarProducto("Huevo Kg", 40, 40, "2025-01-20");
insertarProducto("Papel", 20, 30, "2025-04-05");
insertarProducto("Cafe", 12, 65, "2021-02-28");
insertarProducto("Cocacola", 37, 40, "2024-02-28");

}

void mostrarStock() {
    Producto** productosArray = NULL;
    int cantidadProductos = 0;
    int opcionOrdenamiento;
	system("cls");
    cout << " \t|-------------------------------------------|\n";
    cout << " \t|                                           |\n";
    cout << " \t|  1. FILTRAR POR FECHA DE CADUCIDAD        |\n";
    cout << " \t|  2. FILTRAR POR PRECIO                    |\n";
    cout << " \t|  3. FILTRAR POR CANTIDAD                  |\n";
    cout << " \t|  4. PRODUCTOS VENCIDOS                    |\n";
    cout << " \t|  5. REGRESAR AL MENU ANTERIOR             |\n";
    cout << " \t|                                           |\n";
    cout << " \t|-------------------------------------------|\n";
    cout << "\t Ingrese opcion: ";
    cin >> opcionOrdenamiento;

    switch (opcionOrdenamiento) {
        case 1: //quicksort
        
            productosArray = new Producto*[cantidadProductos];
            orCaducidad(productosArray, 0, cantidadProductos - 1); 
            mostrarOrdenCaducidad();
            delete[] productosArray;
            break;
        case 2:
            orPrecio(); //selecciopcionn directa
            break;
        case 3:
            orCantidad(); //insercion
            break;
        case 4:
	        vencimiento(); 
	        break;            
        case 5:
            return; // Regresar al menú anterior
        default:
            cout << "\tOpcion no valida." << endl;
            break;
    }
}
//METODO DE ORDEMANIENTO POR QUICKSORT
void orCaducidad(Producto* A[], int primero, int ultimo) //QUICKSORT
{
    if (primero < ultimo) {
        int central, i, j;
        Producto* pivote;
        Producto* temporal;
        central = (primero + ultimo) / 2;
        pivote = A[central];
        i = primero;
        j = ultimo;

        do {
            while (A[i]->fechaCaducidad < pivote->fechaCaducidad) i++;
            while (A[j]->fechaCaducidad > pivote->fechaCaducidad) j--;

            if (i <= j) {
                temporal = A[i];
                A[i] = A[j];
                A[j] = temporal;
                i++;
                j--;
            }
        } while (i <= j);

        if (primero < j) {
            orCaducidad(A, primero, j);
        }
        if (ultimo > i) {
            orCaducidad(A, i, ultimo);
        }
    }
}

void mostrarOrdenCaducidad() {
	system("cls");
    Producto* actual = primero;
    int cantidadProductos = 0;

    while (actual != NULL) {
        cantidadProductos++;
        actual = actual->siguiente;
    }

    //se convitre la lista en arrelglo
    Producto** productosArray = new Producto*[cantidadProductos];
    actual = primero;
    int i = 0;

    while (actual != NULL) {
        productosArray[i] = actual;
        actual = actual->siguiente;
        i++;
    }
    orCaducidad(productosArray, 0, cantidadProductos - 1);

	cout << "\t--------------------------------------------------------------------\n";
	cout << "\t| ID | Producto           | Fecha de Caducidad | Cantidad | Precio |\n";
	cout << "\t--------------------------------------------------------------------\n";
	for (int i = 0; i < cantidadProductos; i++) {
	    cout << "\t| " << setw(2) << right << productosArray[i]->id << " | "
	         << setw(18) << left << productosArray[i]->nombre << " | "
	         << setw(18) << right << productosArray[i]->fechaCaducidad << " | "
	         << setw(8) << right << productosArray[i]->cantidad << " | "
	         << setw(6) << right << fixed << setprecision(2) << productosArray[i]->precio << " |\n";
	}
	cout << "\t--------------------------------------------------------------------\n";

    delete[] productosArray;
}

//METODO DE ORDEMANIENTO POR SELECCION DIRECTA
void orPrecio() { 
	system("cls");
    Producto* actual = primero;  
    Producto* siguiente = NULL;
    int cantidadProductos = 0;

    while (actual != NULL) {
        cantidadProductos++;
        actual = actual->siguiente;
    }

    actual = primero; //reinciiar list

    float* precios = new float[cantidadProductos];
    int i = 0;
    while (actual != NULL) {
        precios[i++] = actual->precio;
        actual = actual->siguiente;
    }

    //empieza el metodo
    int min;
    float aux;
    for (int i = 0; i < cantidadProductos; i++) {
        min = i;
        for (int j = i + 1; j < cantidadProductos; j++) {
            if (precios[j] < precios[min]) {
                min = j;
            }
        }
        aux = precios[i];
        precios[i] = precios[min];
        precios[min] = aux;
    }

//ordenados
    actual = primero;
    cout << "\t--------------------------------------------------------------------\n";
	cout << "\t| ID | Producto           | Precio | Cantidad | Fecha de Caducidad |\n";
	cout << "\t-------------------------------------------------------------------\n";
	
	for (int i = 0; i < cantidadProductos; i++) {
	    while (actual != NULL) {
	        if (actual->precio == precios[i]) {
	            cout << "\t| " << setw(2) << right << actual->id << " | "
	                 << setw(18) << left << actual->nombre << " | "
	                 << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
	                 << setw(8) << right << actual->cantidad << " | "
	                 << setw(18) << right << actual->fechaCaducidad << " |\n";
	        }
	        actual = actual->siguiente;
	    }
	    actual = primero; 
	}
	cout << "\t------------------------------------------------------------------\n";

    delete[] precios; 
}

//Lista
void mostrarLista() {
	system("cls");
    Producto* actual = primero;
    cout << "\t--------------------------------------------------------------------\n";
	cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad |\n";
	cout << "\t--------------------------------------------------------------------\n";
	while (actual != NULL) {
	    cout << "\t| " << setw(2) << right << actual->id << " | "
	         << setw(18) << left << actual->nombre << " | "
	         << setw(8) << right << actual->cantidad << " | "
	         << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
	         << setw(18) << right << actual->fechaCaducidad << " |\n";
	
	    actual = actual->siguiente;
	}
	cout << "\t---------------------------------------------------------------\n";
}

////METODO DE ORDEMANIENTO POR INSERCION
void orCantidad() {
    if (primero == NULL || primero->siguiente == NULL) {
        cout << "\tLa lista esta vacía o tiene un solo elemento. ." << endl;
        return;
    }

    Producto* sorted = NULL;
    Producto* current = primero;

    while (current != NULL) {
        Producto* next = current->siguiente;

        if (sorted == NULL || sorted->cantidad >= current->cantidad) {
            current->siguiente = sorted;
            sorted = current;
        } else {
            Producto* temp = sorted;
            while (temp->siguiente != NULL && temp->siguiente->cantidad < current->cantidad) {
                temp = temp->siguiente;
            }
            current->siguiente = temp->siguiente;
            temp->siguiente = current;
        }

        current = next;
    }

    primero = sorted;

    cout << "\nOrden ascendente por cantidad: ";
    mostrarLista();

    //system("pause");
}

void modificarProducto() {
	system("cls");
    Producto* actual = primero;
    bool encontrado = false;
    int idBuscar = 0;
	mostrarLista();
    cout << "\n\tIngrese el ID del producto a modificar: ";
    cin >> idBuscar;
	system("cls");
    while (actual != NULL && !encontrado) {
        if (actual->id == idBuscar) {
            cout << "\n\t Producto encontrado:\n";
            
            cout << "\t--------------------------------------------------------------------\n";
			cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad |\n";
			cout << "\t--------------------------------------------------------------------\n";
			cout << "\t| " << setw(2) << right << actual->id << " | "
			     << setw(18) << left << actual->nombre << " | "
			     << setw(8) << right << actual->cantidad << " | "
			     << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
			     << setw(18) << right << actual->fechaCaducidad << " |\n";
			cout << "\t--------------------------------------------------------------------\n";

            cout << "\n\tIngrese el nuevo nombre del producto: ";
			cin >> actual->nombre;
			transform(actual->nombre.begin(), actual->nombre.end(), actual->nombre.begin(), ::toupper);
			cout << "\tIngrese la nueva cantidad: ";
			cin >> actual->cantidad;
			cout << "\tIngrese el nuevo precio: ";
			cin >> actual->precio;
			cout << "\tIngrese la nueva fecha de caducidad (formato: AAAA-MM-DD): ";
			cin >> actual->fechaCaducidad;
			
			cout << "\t\nProducto modificado con exito.\n";
			encontrado = true;

        }
        actual = actual->siguiente;
    }

    if (!encontrado) {
        cout << "\tProducto No Encontrado";
    }
}

void eliminarProducto() {
    Producto* actual = primero;
    Producto* anterior = NULL;
    bool encontrado = false;
    int idBuscar = 0;
	mostrarLista();
    cout << "\n\tIngrese el ID del producto a modificar: ";
    cin >> idBuscar;
    system("cls");
    while (actual != NULL && !encontrado) {
        if (actual->id == idBuscar) {
            cout << "\t------------------------------------------------------------------\n";
			cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad |\n";
			cout << "\t--------------------------------------------------------------------\n";
			cout << "\t| " << setw(2) << right << actual->id << " | "
			     << setw(18) << left << actual->nombre << " | "
			     << setw(8) << right << actual->cantidad << " | "
			     << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
			     << setw(18) << right << actual->fechaCaducidad << " |\n";
			cout << "\t-------------------------------------------------------------------\n";

            int opcion;
            cout << "\t1. Eliminar producto del inventario\n";
            cout << "\t2. Eliminar unidades\n";
			cout << "\tSeleccione la opcion: ";
            cin >> opcion;

            if (opcion == 1) {
                // Eliminar el producto completo
                if (actual == primero) {
                    primero = primero->siguiente;
                } else if (actual == ultimo) {
                    anterior->siguiente = NULL;
                    ultimo = anterior;
                } else {
                    anterior->siguiente = actual->siguiente;
                }

                delete actual;
                cout << "\nProducto eliminado con exito.\n";
            } else if (opcion == 2) {
                // Eliminar unidades
                int unidadesEliminar;
                cout << "\tIngrese la cantidad de unidades a eliminar: ";
                cin >> unidadesEliminar;

                if (unidadesEliminar <= actual->cantidad) {
                    actual->cantidad -= unidadesEliminar;
                    cout << "\nUnidades eliminadas con exito.\n";
                } else {
                    cout << "\nError: La cantidad a eliminar es mayor que la cantidad actual.\n";
                }
               
            } else {
                cout << "\nOpcion no valida.\n";
            }

            encontrado = true;
        }

        anterior = actual;
        actual = actual->siguiente;
    }

    if (!encontrado) {
        cout << "Producto no encontrado.\n";
    }
}


//ORDENAMIENTO METODO RADIXSORT
void ordradixsort(Producto*& inicio) {
    auto getMaxIdLength = [](Producto* inicio) -> int {
        int max = 0;
        Producto* temp = inicio;
        while (temp != NULL) {
            int numDigitos = static_cast<int>(log10(abs(temp->id)) + 1);
            if (numDigitos > max) {
                max = numDigitos;
            }
            temp = temp->siguiente;
        }
        return max;
    };

    int maxDigitCount = getMaxIdLength(inicio);

    //Radixsort
    for (int d = 0; d < maxDigitCount; ++d) {
        Producto* *contenedores = new Producto*[10] { NULL };

        while (inicio != NULL) {
            int digito = (abs(inicio->id) / static_cast<int>(pow(10, d))) % 10;
            Producto* &contenedor = contenedores[digito];
            if (contenedor == NULL) {
                contenedor = inicio;
            } else {
                Producto* temp = contenedor;
                while (temp->siguiente != NULL) {
                    temp = temp->siguiente;
                }
                temp->siguiente = inicio;
            }
            Producto* siguiente = inicio->siguiente;
            inicio->siguiente = NULL;
            inicio = siguiente;
        }

        Producto* *finContenedor = &inicio;
        for (int i = 0; i < 10; ++i) {
            Producto* contenedor = contenedores[i];
            while (contenedor != NULL) {
                *finContenedor = contenedor;
                finContenedor = &(contenedor->siguiente);
                contenedor = contenedor->siguiente;
            }
        }

        delete[] contenedores;
    }
}

//BUSQUEDA BINARA PARA ENCONTRAR ID
Producto* binariaId(int idBuscar) {
    ordradixsort(primero);

    Producto* inicio = primero;

    int inicioLista = 0;
    int finLista = 0;

    while (inicio != NULL) {
        finLista++;
        inicio = inicio->siguiente;
    }

    inicio = primero;
	system("cls");
    while (inicioLista < finLista) {
        int mitad = inicioLista + (finLista - inicioLista) / 2;

        Producto* temp = inicio;
        int posMitad = 0;

        while (temp != NULL && posMitad < mitad) {
            temp = temp->siguiente;
            posMitad++;
        }
		
        if (temp != NULL && temp->id == idBuscar) {
            cout << "\n\tProducto encontrado: " << temp->id << endl;
            return temp;
        }

        if (temp != NULL && temp->id > idBuscar) {
            finLista = mitad;
        } else {
            inicioLista = mitad + 1;
        }
    }
    cout << "\n\tProducto no encontrado." << endl;
    return NULL;
}

//IMPLEMENTACION DE PILA
void vencimiento() {
	system("cls");
    Producto* actual = primero;
    time_t now = time(NULL);

    cout << "\tProductos vencidos:\n";
    cout << "\t-------------------------------------------------------------------------------|\n";
	cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad | Estado    |\n";
	cout << "\t-------------------------------------------------------------------------------|\n";    
    while (actual != NULL) {
        tm fechaCaducidadTM = {};
        if (sscanf(actual->fechaCaducidad.c_str(), "%d-%d-%d",
                        &fechaCaducidadTM.tm_year, &fechaCaducidadTM.tm_mon, &fechaCaducidadTM.tm_mday) != 3) {
            actual = actual->siguiente;
            continue;
        }

        fechaCaducidadTM.tm_year -= 1900;
        fechaCaducidadTM.tm_mon--;

        time_t fechaCaducidad = mktime(&fechaCaducidadTM);

        tm nowTM = *localtime(&now);
        nowTM.tm_hour = 0;
        nowTM.tm_min = 0;
        nowTM.tm_sec = 0;
        time_t today = mktime(&nowTM);

        if (today >= fechaCaducidad) {

			cout << "\t| " << setw(2) << right << actual->id << " | "
			     << setw(18) << left << actual->nombre << " | "
			     << setw(8) << right << actual->cantidad << " | "
			     << setw(6) << right << fixed << setprecision(2) << actual->precio << " | "
			     << setw(18) << right << actual->fechaCaducidad << " | VENCIDO   |\n";

            NodoPila* temp = new NodoPila;
            temp->producto = actual;
            temp->siguiente = c;
            c = temp;
        }
		
        actual = actual->siguiente;
    }
    	cout << "\t|-------------------------------------------------------------------------------|\n";

}

void pilavencidos(void) {
    NodoPila* temp = c;
    cout << "\nProductos vencidos en la pila:\n";
    while (temp) {
        cout << "ID: " << temp->producto->id << ", Nombre: " << temp->producto->nombre << endl;
        temp = temp->siguiente;
    }
}

void obtenerListaCiudades(NodoArbol* raiz, vector<string>& listaCiudades) {
    if (raiz != NULL) {
        obtenerListaCiudades(raiz->izquierda, listaCiudades);
        listaCiudades.push_back(raiz->ciudad);
        obtenerListaCiudades(raiz->derecha, listaCiudades);
    }
}

bool vender(NodoArbol* arbolCiudades) {
	system("cls");
    int idProducto;
    lisordenada();
    cout << "\tIngrese el ID del producto a vender: ";
    cin >> idProducto;
    Producto* producto = binariaId(idProducto);

    if (producto != NULL) {
        vector<string> listaCiudades;
        obtenerListaCiudades(arbolCiudades, listaCiudades);

        for (int i = 0; i < listaCiudades.size(); ++i) {
            cout << "\t"<<i + 1 << ". " << listaCiudades[i] << "\n";
        }
        
        int opcionDestino;
        cout << "\tIngrese la ciudad de destino (0 para cancelar): ";
        cin >> opcionDestino;

        if (opcionDestino >= 1 && opcionDestino <= listaCiudades.size()) {
            string ciudadDestino = listaCiudades[opcionDestino - 1];

            // Buscar la ciudad en el arbol
            NodoArbol* ciudadEncontrada = buscarCiudad(arbolCiudades, ciudadDestino);

            if (ciudadEncontrada != NULL) {
				cout << "\n\t---Producto vendido a la ciudad de: " << ciudadDestino <<"---\n\n";
                mostrarRuta(arbolCiudades, ciudadDestino);
                
				system("pause");
                return true;
            } else {
                cout << "\tCiudad no encontrada.\n";
            }
        } else if (opcionDestino != 0) {
            cout << "\tOpcion no valida. La venta ha sido cancelada.\n";
        }
    } else {
        cout << "Producto no encontrado.\n";
    }

    return false;
}

void mostrarRuta(NodoArbol* arbolCiudades, const string& ciudadEncontrada) {
    vector<string> ruta;
    obtenerRuta(arbolCiudades, ciudadEncontrada, ruta);

    cout << "Ruta desde la Yuriria hasta " << ciudadEncontrada << ":\n";
    for (const auto& ciudad : ruta) {
        cout <<ciudad<< "<-";
    }
    cout << "Origen\n\n\n";
}
bool obtenerRuta(NodoArbol* raiz, const string& destino, vector<string>& ruta) {
    if (raiz != NULL) {
        if (raiz->ciudad == destino) {
            ruta.push_back(raiz->ciudad);
            return true;
        }

        if (obtenerRuta(raiz->izquierda, destino, ruta) || obtenerRuta(raiz->derecha, destino, ruta)) {
            ruta.push_back(raiz->ciudad);
            return true;
        }
    }

    return false;
}


NodoArbol* buscarCiudad(NodoArbol* raiz, const string& ciudad) {
    if (raiz == NULL || raiz->ciudad == ciudad) {
        return raiz;
    }

    // Buscar en el subarbol izquierdo
    NodoArbol* izquierda = buscarCiudad(raiz->izquierda, ciudad);
    if (izquierda != NULL) {
        return izquierda;
    }

    // Buscar en el subarbol derecho
    return buscarCiudad(raiz->derecha, ciudad);
}

//MENUS
void menuprod(){
	ProductosIniciales(); 
    ordradixsort(primero);
    NodoArbol* arbolCiudades = new NodoArbol("Yuriria");

    //hijos derechs
    arbolCiudades->izquierda = new NodoArbol("Uriangato");
	arbolCiudades->izquierda->izquierda = new NodoArbol("Moroleon");
	arbolCiudades->izquierda->derecha = new NodoArbol("Morelia");
    //hijos izq
    arbolCiudades->derecha = new NodoArbol("Casacuaran");
    arbolCiudades->derecha->derecha = new NodoArbol("Salvatierra");
    int op;
    do {
        menustock();
        cin >> op;
        cin.ignore(numeric_limits<streamsize>::max(), '\n'); 

        switch (op) {
            case 1:
            	system("cls");
		        cout << "\t|-------------------------------------------|\n";
		        cout << "\t|             INSERTAR PRODUCTO             |\n";
		        cout << "\t|-------------------------------------------|\n";
                insertarProducto();
                cout << "\n\n Producto ingresado"<<endl;
                break;

            case 2: {
                system("cls");
				int idABuscar;
				cout << "\t|-------------------------------------------|\n";
		        cout << "\t|              BUSCAR PRODUCTO              |\n";
		        cout << "\t|-------------------------------------------|\n";
                cout << "\tIngrese el ID del producto a buscar: ";
                cin >> idABuscar;
				
                Producto* encontrado = binariaId(idABuscar);

                if (encontrado != NULL) {
                    //cout << "Producto encontrado:\n";
					cout << "\t-------------------------------------------------------------------\n";
					cout << "\t| ID | Producto           | Cantidad | Precio | Fecha de Caducidad |\n";
					cout << "\t-------------------------------------------------------------------\n";
					cout << "\t| " << setw(2) << right << encontrado->id << " | "
					     << setw(18) << left << encontrado->nombre << " | "
					     << setw(8) << right << encontrado->cantidad << " | "
					     << setw(6) << right << fixed << setprecision(2) << encontrado->precio << " | "
					     << setw(18) << right << encontrado->fechaCaducidad << " |\n";
					cout << "\t-------------------------------------------------------------------|\n";
                } else {
                    cout << "Producto no encontrado.\n";
                }
                }
                break;
            
            case 3:
				cout << "\t|-------------------------------------------|\n";
				cout << "\t|             MODIFICAR PRODUCTO            |\n";
				cout << "\t|-------------------------------------------|\n";
                modificarProducto();
                break;

            case 4:
            	system("cls");
                lisordenada();
                eliminarProducto();
                break;

            case 5:       	
                mostrarStock();
                break;
            case 6: 
                vender(arbolCiudades);
                break;
            case 7:
                cout << "Hasta Luego";
                menuprin();
                break;

            default:
                cout << "Opcion No Valida ";
        }
        cout << endl << endl;
        system("pause");
    } while (op != 7);
}

void menustock() {
	system("cls");
    cout << " \t|-------------------------------------------|\n";
    cout << " \t|                    STOCK                  |\n";
    cout << " \t|-------------------------------------------|\n";
    cout << " \t|                                           |\n";
    cout << " \t|  1. INSERTAR PRODUCTO                     |\n";
    cout << " \t|  2. BUSCAR PRODUCTO                       |\n";
    cout << " \t|  3. MODIFICAR                             |\n";
    cout << " \t|  4. ELIMINAR PRODUCTO                     |\n";
    cout << " \t|  5. MOSTRAR STOCK                         |\n";
    cout << " \t|  6. VENDER A DOMICILIO                    |\n";    
    cout << " \t|  7. Regresar al menu                      |\n";
    cout << " \t|                                           |\n";
    cout << " \t|-------------------------------------------|\n";
    cout << "\t Ingrese una opcion: ";
}

void menuprin(){
	int opc;
    do {
    	system("cls");
        cout << "\n\t|-------------------------------------------|\n";
        cout << "\t|          ADMINISTRACION DE TIENDA         |\n";
        cout << "\t|-------------------------------------------|\n";
        cout << "\t|                                           |\n";
        cout << "\t|  1. ADMINISTRAR PRODUCTOS                 |\n";
        cout << "\t|  2. ADMINISTRAR CAJEROS                   |\n";
        cout << "\t|  3. Salir                                 |\n";
        cout << "\t|                                           |\n";
        cout << "\t|-------------------------------------------|\n";
        cout << "\tIngrese una opcion: ";
        cin >> opc;
        switch (opc) {
            case 1:
            menuprod();
                break;
            case 2:
            menuCajero();           	
                break;
            case 3:
                cout << "Hasta luego.\n";
                exit(0);
            default:
                cout << "Opcion no valida." << endl;
        }
    } while (opc != 3);
}
///////////////////////CAJEROS//////////////////////////////
void menuCajero() {
    struct cola colaSueldos;  
    colaSueldos.delante = NULL;
    colaSueldos.atras = NULL;
    int opc;
    do {
    	system("cls");
        cout << "\t|-------------------------------------------|\n";
        cout << "\t|                   CAJEROS                 |\n";
        cout << "\t|-------------------------------------------|\n";
        cout << "\t|                                           |\n";
        cout << "\t|  1. Agregar Cajero                        |\n";
        cout << "\t|  2. Buscar Cajero por ID                  |\n";
        cout << "\t|  3. Buscar Cajero por Nombre              |\n";
        cout << "\t|  4. Modificar Cajero                      |\n";
        cout << "\t|  5. Eliminar Cajero                       |\n";
        cout << "\t|  6. Mostrar Cajeros                       |\n";
        cout << "\t|  7. Sueldos                               |\n";
        cout << "\t|  8. Regresar al menu                      |\n";
        cout << "\t|                                           |\n";
        cout << "\t|-------------------------------------------|\n";
        cout << "\tIngrese una opcion: ";
        cin >> opc;
        switch (opc) {
            case 1:
                agregarCajero();
                break;
            case 2:
                buscarCajeroID();
                break;
            case 3: 
                buscarCajeroNombre();
                break;
            case 4:          	
            	system("cls");
                modificarCajero();
                system("pause");
                break;
            case 5:
                eliminarCajero();
                break;
            case 6:
            	system("cls");
                mostrarCajeros();
                system("pause");
                break;
            case 7:
                limpiarColaSueldos(colaSueldos);
                calcularSueldos(colaCajeros, colaSueldos);
                mostrarSueldos(colaSueldos);
                break;
            case 8:
                menuprin();
            default:
                cout << "Opcion no valida." << endl;
        }
    } while (opc != 9);
}

int stringtokey(const char* str) {
    int key = 0;
    for (int i = 0; str[i] != '\0'; i++) {
        key += str[i];
    }
    return key;
}

int buscarNodo(char nName[]) {
    int ascii = stringtokey(nName);
    int key = ascii % tablesize;
    node* n = HashTable[key];

    while (n != NULL) {
        if (strcmp(n->Names, nName) == 0) {
            return key;
        }
        n = n->next;
    }

    return -1;
}

int aleid() {
    contadorId++;  
    return contadorId;
}

void agregarCajero() {
	system("cls");
    Cajero* nuevo = new Cajero();
    nuevo->id = aleid();
    cout << "\n\n\tIngrese el nombre del cajero: ";
    cin.ignore();
    getline(cin, nuevo->nombre);
	transform(nuevo->nombre.begin(), nuevo->nombre.end(), nuevo->nombre.begin(), ::toupper);

    bool numeroValido = false;
    do {
        cout << "\tIngrese el numero de telefono (deben ser 10 digitos): ";
        cin >> nuevo->telefono;

        if (nuevo->telefono.length() == 10) {
            numeroValido = true;
        } else {
            cout << "\tNumero no valido. Debe contener exactamente 10 digitos. Intentelo de nuevo." << endl;
        }
    } while (!numeroValido);

    int opcionSexo;
    cout << "\tSeleccione el sexo:\n";
    cout << "\t1. Mujer\n";
    cout << "\t2. Hombre\n";
    cout << "\t3. No definido\n";
    cout << "\tOpcion: ";
    cin >> opcionSexo;

    if (opcionSexo == 1) {
        nuevo->sexo = "Mujer";
    } else if (opcionSexo == 2) {
        nuevo->sexo = "Hombre";
    } else if (opcionSexo == 3) {
        nuevo->sexo = "No definido";
    } else {
        cout << "\tOpcion no valida. Seleccionando 'No definido' por defecto." << endl;
        nuevo->sexo = "No definido";
    }

    cout << "\tIngrese la antiguedad para " << nuevo->nombre << ": ";
    cin >> nuevo->antiguedad;

    if (nuevo->antiguedad < 0) {
        cout << "Antiguedad no valida. Se establecera a 0." << endl;
        nuevo->antiguedad = 0;
    }

    nodo* nuevoNodo = new nodo;
    nuevoNodo->cajero = nuevo;
    nuevoNodo->sgte = NULL;

    if (colaCajeros.delante == NULL) {
        colaCajeros.delante = nuevoNodo;
        colaCajeros.atras = nuevoNodo;
    } else {
        colaCajeros.atras->sgte = nuevoNodo;
        colaCajeros.atras = nuevoNodo;
    }

    if (primeroCaj == NULL) {
        primeroCaj = nuevo;
        ultimoCaj = nuevo;
    } else {
        nuevo->anterior = ultimoCaj;
        ultimoCaj->siguiente = nuevo;
        ultimoCaj = nuevo;
    }

    int key = stringtokey(nuevo->nombre.c_str()) % tablesize;
    node* nuevoNode = new node;
    strcpy(nuevoNode->Names, nuevo->nombre.c_str());
    nuevoNode->next = HashTable[key];
    HashTable[key] = nuevoNode;

    cout << "\n\tCajero con ID " << nuevo->id << " agregado con exito" << endl;
    system("pause");
	system("cls");
}

//BUSQUEDA SECUENCIAL
void buscarCajeroID() {
	system("cls");
    if (primeroCaj != NULL) {
        int idBuscar;
        cout << "\n\n\tIngrese el ID del cajero a buscar: ";
        cin >> idBuscar;
        Cajero* actual = primeroCaj;
        int posicion = 1;
        bool encontrado = false;

        while (actual != NULL) {
            if (actual->id == idBuscar) {
                cout << "\t------------------------------------------------------------\n";
                cout << "\t| ID   | Nombre           | Telefono       | Sexo          |\n";
                cout << "\t------------------------------------------------------------\n";
                cout << "\t| " << setw(5) << left << actual->id << " | "
                     << setw(15) << left << actual->nombre << " | "
                     << setw(14) << left << actual->telefono << " | "
                     << setw(13) << left << actual->sexo << " |\n";
                cout << "\t-----------------------------------------------------------\n";

                encontrado = true;
                system("pause");
                break;
            }
            actual = actual->siguiente;
            posicion++; 
        }
        if (!encontrado) {
            cout << "\tCajero no encontrado." << endl;
        }
    } else {
        cout << "\tLa lista de cajeros esta vacia." << endl;
    }
}

//HASHING
void buscarCajeroNombre() {
    system("cls");
	cin.ignore();
    cout << "\n\n\tIntroduzca el Nombre a buscar: ";
    char nName[256];
    cin.getline(nName, 256);
	transform(nName, nName + strlen(nName), nName, ::toupper);


    int key = buscarNodo(nName);
    if (key == -1) {
        cout << "\tEl nombre no se encuentra en la lista de empleados." << endl;
        system ("pause");
    } else {
        cout << "\tEl nombre se encuentra en la lista de empleados." << endl;

            Cajero* actual = primeroCaj;
            while (actual != NULL) {
                if (actual->nombre == nName) {
                    cout << "\t------------------------------------------------------------\n";
                    cout << "\t| ID   | Nombre           | Telefono       | Sexo          |\n";
                    cout << "\t-----------------------------------------------------------\n";
                    cout << "\t| " << setw(5) << left << actual->id << " | "
                         << setw(15) << left << actual->nombre << " | "
                         << setw(14) << left << actual->telefono << " | "
                         << setw(13) << left << actual->sexo << " |\n";
                    cout << "\t-----------------------------------------------------------\n";
                    system("pause");
                    break;
                }
                actual = actual->siguiente;
            }
        
    }
}

void modificarCajero() {
    if (primeroCaj != NULL) {
        int idModificar;
        mostrarCajeros();
        cout << "\n\n\tIngrese el ID del cajero a modificar: ";
        cin >> idModificar;
        Cajero* actual = primeroCaj;
        bool encontrado = false;
		system("cls");
        while (actual != NULL) {
            if (actual->id == idModificar) {
                cout << "\t------------------------------------------------------------\n";
                cout << "\t| ID   | Nombre           | Telefono       | Sexo          |\n";
                cout << "\t------------------------------------------------------------\n";
                cout << "\t| " << setw(5) << left << actual->id << " | "
                     << setw(15) << left << actual->nombre << " | "
                     << setw(14) << left << actual->telefono << " | "
                     << setw(13) << left << actual->sexo << " |\n";
                cout << "\t------------------------------------------------------------\n";


                cout << "\tIngrese el nuevo nombre del cajero: ";
                cin.ignore();
                getline(cin, actual->nombre);
                transform(actual->nombre.begin(), actual->nombre.end(), actual->nombre.begin(), ::toupper);
                
                cout << "\tIngrese el nuevo numero de telefono: ";
                cin >> actual->telefono;
                
                int opcionSexo;
                cout << "\tSeleccione el nuevo sexo:\n";
                cout << "\t1. Mujer\n";
                cout << "\t2. Hombre\n";
                cout << "\t3. No definido\n";
                cout << "\tOpcion: ";
                cin >> opcionSexo;

                if (opcionSexo == 1) {
                    actual->sexo = "Mujer";
                } else if (opcionSexo == 2) {
                    actual->sexo = "Hombre";
                } else if (opcionSexo == 3) {
                    actual->sexo = "No definido";
                } else {
                    cout << "\tOpcion no valida. Seleccionando 'No definido' por defecto." << endl;
                    actual->sexo = "No definido";
                }

                cout << "\tCajero modificado con exito." << endl;
                encontrado = true;
                break;
            }
            actual = actual->siguiente;
        }

        if (!encontrado) {
            cout << "\tCajero no encontrado." << endl;
        }
    } else {
        cout << "\tLa lista de cajeros esta vacia." << endl;
    }
}

void eliminarCajero() {
	system("cls");
    if (primeroCaj != NULL) {
        int idEliminar;
        cout << "\n\n\tIngrese el ID del cajero a eliminar: ";
        cin >> idEliminar;
        Cajero* actual = primeroCaj;
        Cajero* anterior = NULL;
        bool encontrado = false;

        while (actual != NULL) {
            if (actual->id == idEliminar) {
                cout << "Cajero encontrado: ID: " << actual->id << ", Nombre: " << actual->nombre
                    << ", Telefono: " << actual->telefono << ", Sexo: " << actual->sexo << endl;

                if (actual == primeroCaj) {
                    primeroCaj = primeroCaj->siguiente;
                    if (primeroCaj != NULL) {
                        primeroCaj->anterior = NULL;
                    }
                } else if (actual == ultimoCaj) {
                    anterior->siguiente = NULL;
                    ultimoCaj = anterior;
                } else {
                    anterior->siguiente = actual->siguiente;
                    actual->siguiente->anterior = anterior;
                }
				
                delete actual;
                system("pause");
                cout << "\tCajero eliminado con exito." << endl;
                encontrado = true;
                break;
            }
            anterior = actual;
            actual = actual->siguiente;
        }

        if (!encontrado) {
            cout << "\tCajero no encontrado." << endl;
        }
    } else {
        cout << "\tLa lista de cajeros esta vacia." << endl;
    }
}
//METODO DE ORDENAMIENTO POR BURBUJA
void mostrarCajeros() {
    if (primeroCaj == NULL) {
        cout << "\tLa lista de cajeros está vacía." << endl;
        return;
    }

    int n = 0;
    Cajero* actual = primeroCaj;
    while (actual != NULL) {
        n++;
        actual = actual->siguiente;
    }
	//metodo intercambio directo
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            Cajero* actual = primeroCaj;
            for (int k = 0; k < j; k++) {
                actual = actual->siguiente;
            }
            Cajero* siguiente = actual->siguiente;

            if (actual->id > siguiente->id) {
                //intercambio
                swap(actual->id, siguiente->id);
                swap(actual->nombre, siguiente->nombre);
                swap(actual->telefono, siguiente->telefono);
                swap(actual->sexo, siguiente->sexo);
            }
        }
    }

    actual = primeroCaj;
    cout << "\t------------------------------------------------------------\n";
    cout << "\t| ID   | Nombre           | Telefono       | Sexo          |\n";
    cout << "\t------------------------------------------------------------\n";
    while (actual != NULL) {
        cout << "\t| " << setw(5) << left << actual->id << " | "
             << setw(15) << left << actual->nombre << " | "
             << setw(14) << left << actual->telefono << " | "
             << setw(13) << left << actual->sexo << " |\n";
        cout << "\t-----------------------------------------------------------\n";

        actual = actual->siguiente;
    }
}

void verSueldo(Cajero* cajero) {
    int aniosAntiguedad = cajero->antiguedad;

    cout << "\t\n\nEl sueldo del cajero " << cajero->nombre << " es: $" << cajero->sueldoInfo.sueldo << endl;

    if (aniosAntiguedad < 1) {
        cout << "\tMenos de un año de antiguedad." << endl;
    } else if (aniosAntiguedad == 1) {
        cout << "\tUn año de antiguedad." << endl;
    } else {
        cout << "\tMas de un año de antiguedad." << endl;
    }
}


//COLAS DE PRIORIDAD
void ordenarPrioridad(struct cola &q) {
    set<pair<float, Cajero*>> cajerosPorAntiguedad;

    struct nodo *aux = q.delante;
    while (aux != NULL) {
        cajerosPorAntiguedad.insert({aux->cajero->antiguedad, aux->cajero});
        aux = aux->sgte;
    }

    while (q.delante != NULL) {
        struct nodo *temp = q.delante;
        q.delante = temp->sgte;
        delete temp;
    }
    q.atras = NULL;

    for (const auto& pair : cajerosPorAntiguedad) {
        Cajero* cajero = pair.second;
        nodo* nuevoNodo = new nodo;
        nuevoNodo->cajero = cajero;
        nuevoNodo->sgte = NULL;

        if (q.delante == NULL) {
            q.delante = nuevoNodo;
            q.atras = nuevoNodo;
        } else {
            q.atras->sgte = nuevoNodo;
            q.atras = nuevoNodo;
        }
    }
}

void mostrarCola(struct cola q) {
    struct nodo *aux;
    aux = q.delante;
    cout << "\tNombre|Antiguedad|Sueldo" << endl;
    cout << "\t------------------------" << endl;
    while (aux != NULL) {
        Cajero *cajero = aux->cajero;
        cout << " " << cajero->nombre << " | " << cajero->antiguedad << " | " << cajero->sueldoInfo.sueldo << endl;
        aux = aux->sgte;
    }
}

void calcularSueldos(struct cola &q, struct cola &colaSueldos) {
    // Ordenar la cola por prioridad de antiguedad
    ordenarPrioridad(q);

    struct nodo *aux;
    aux = q.delante;

    while (aux != NULL) {
        // Sueldo
        Cajero *cajero = aux->cajero;

        const int sueldoBase = 1500;
        const int aumentoAnual = 150;
        cajero->sueldoInfo.sueldo = sueldoBase + (cajero->antiguedad > 5 ? 5 : cajero->antiguedad) * aumentoAnual;

        nodo* nuevoNodo = new nodo;
        nuevoNodo->cajero = cajero;
        nuevoNodo->sgte = NULL;

        if (colaSueldos.delante == NULL) {
            colaSueldos.delante = nuevoNodo;
            colaSueldos.atras = nuevoNodo;
        } else {
            colaSueldos.atras->sgte = nuevoNodo;
            colaSueldos.atras = nuevoNodo;
        }

        aux = aux->sgte;
    }
}

void mostrarSueldos(struct cola &colaSueldos) {
    system("cls");
    cout << "\t----------------------------------------------------------\n";
    cout << "\t| ID   | Nombre           | Antiguedad |      Sueldo     |\n";
    cout << "\t----------------------------------------------------------\n";

    struct nodo *aux = colaSueldos.delante;
    while (aux != NULL) {
        Cajero* cajero = aux->cajero;
        cout << "\t| " << setw(5) << left << cajero->id << " |"
             << setw(15) << left << cajero->nombre << "  |"
             << setw(10) << left << cajero->antiguedad << "  |"
             << setw(13) << right << fixed << setprecision(2) << "$"<<cajero->sueldoInfo.sueldo << "|\n";
        aux = aux->sgte;
    }

    cout << "\t----------------------------------------------------------\n";
    system("pause");
    system("cls");
}

void limpiarColaSueldos(struct cola &colaSueldos) {
    while (colaSueldos.delante != NULL) {
        struct nodo *temp = colaSueldos.delante;
        colaSueldos.delante = temp->sgte;
        delete temp;
    }
    colaSueldos.atras = NULL;
}
