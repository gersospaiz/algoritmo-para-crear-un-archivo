# algoritmo-para-crear-un-archivo
lo que hace el programa es que guarda datos y si queremos lo podemos exportar a un archivo .txt
#include <iostream>
#include <string>
#include <iomanip>
#include <algorithm>
#include <vector>
#include <cctype>
#include <ctime>
#include <fstream>

using namespace std;

struct nuevo_estudiante {
    string nombre;
    int edad;
    int nota;
    vector<int> calificaciones; // ESTO ES PARA TENER EL VECTOR Y SE PUEDA GUARDAR TODAS LAS NOTAS 
};

int main() {
    system("chcp 65001");
    int opciones;
    int cantidad = 0;
    nuevo_estudiante gestion[100];

    do {
        cout << "1) AGREGAR UN NUEVO ESTUDIANTES" << endl;
        cout << "2) VER TODOS LOS ESTUDIANTES" << endl;
        cout << "3) BUSRCAR ESTUDIANTE POR NOMBRE" << endl;
        cout << "4) ESTUDIANTES CON MEJOR PROMEDIO" << endl;
        cout << "5) CREAR UN ARCHICO CON LOS ESTUDIANTES APROBADOS" << endl;
        cout << "6) ESTADISTICA" << endl;
        cout << "0) QUIERE SALIR?" << endl;

        cin >> opciones;
        cin.ignore();

        switch (opciones) {
            case 1: {
                int cantidad_local;

                cout << "INGRESE LA CANTIDAD DE ESTUDDIANTES QUE QUIERE INGRESAR: ";
                cin >> cantidad_local;
                cin.ignore();

                for (int i = 1; i <= cantidad_local; i++) {
                    nuevo_estudiante estudiantes;

                    cout << "INGRESE EL NOMBRE DEL ESTUDIANTE: " << endl;
                    getline(cin, estudiantes.nombre);
                    cout << "INGRESE LA EDAD DEL ESTUDIANTE: " << endl;
                    cin >> estudiantes.edad;
                    cout << "CUANTAS NOTAS QUIERE INGRESAR:" << endl;
                    cin >> estudiantes.nota;

                    while (estudiantes.nota < 3) {
                        cout << "Debe ingresar por lo menos tres calificaciones:" << endl;
                        cin >> estudiantes.nota;
                    }

                    for (int j = 1; j <= estudiantes.nota; j++) {
                        int nota_individual;
                        do {
                            cout << "INGRESE LA NOTA " << j << " (0 a 10): " << endl;
                            cin >> nota_individual;
                            if (nota_individual < 0 || nota_individual > 10) {
                                cout << "❌ Nota inválida. Debe estar entre 0 y 10." << endl;
                            }
                        } while (nota_individual < 0 || nota_individual > 10);
                        estudiantes.calificaciones.push_back(nota_individual);
                    }

                    cin.ignore();

                    gestion[cantidad] = estudiantes;
                    cantidad++;
                    cout << endl;
                }

                break;
            }

            case 2: {
                cout << "****** VER TODOS LOS ESTUDIANTES *******" << endl;

                if (cantidad == 0) {
                    cout << "TODAVÍA NO SE HAN INGRESADO ESTUDIANTES." << endl;
                } else {
                    ofstream archivo("todos_los_estudiantes.txt", ios::app); // ESTA ES LA LINEA QUE HACE QUE PUEDA CREAR UN ARCHIVO .TXT

                    if (!archivo.is_open()) { // ESTO VALIDA SI ESTA ABIERTO EL ARCHIVO Y DA ERROR 
                        cout << "❌ No se pudo crear el archivo." << endl;
                        break;
                    }

                    archivo << "LISTA COMPLETA DE ESTUDIANTES:\n\n";

                    for (int i = 0; i < cantidad; i++) {
                        archivo << "ESTUDIANTE #" << (i + 1) << endl; // COMO VEN CUANDO QUIERO GUARDAR COSAS EN UN ARCHIVO EN VEZ DE ESCRIBIR COUT SE ESCRIBE ARCHIVO
                        archivo << "NOMBRE: " << gestion[i].nombre << endl;
                        archivo << "EDAD: " << gestion[i].edad << endl;
                        archivo << "NOTAS: ";

                        for (int nota : gestion[i].calificaciones) {
                            archivo << nota << " ";
                        }

                        archivo << "\n----------------------------------------\n";
                    }

                    archivo.close(); // AQUI TERMINA LA FUNCION DEL ARCHIVO
                    cout << "✅ Archivo 'todos_los_estudiantes.txt' creado correctamente.\n";
                }

                break;
            }

            case 3: {
                cout << "*********** BUSRCAR EL ESTUDIANTE POR EL NOMBRE ***************" << endl;

                if (cantidad == 0) {
                    cout << "TODAVIA NO SE HAN INGRESADO LOS ESTUDIANTES " << endl;
                } else {
                    string nombrebuscado;
                    cout << "INGRESE EKL NOMBRE DEL ESTUDIANTE: " << endl;

                    getline(cin, nombrebuscado);

                    string nombrebuscadolow = nombrebuscado;
                    transform(
                        nombrebuscadolow.begin(),
                        nombrebuscadolow.end(),
                        nombrebuscadolow.begin(),
                        ::tolower
                    );

                    bool encontrado = false;

                    for (int i = 0; i < cantidad; i++) {
                        nuevo_estudiante estudiante = gestion[i];
                        string nombreactuallow = estudiante.nombre; // LO QUE HACE ES TRANSFORMAR LAS PALABRAS 
                        transform(
                            nombreactuallow.begin(),
                            nombreactuallow.end(),
                            nombreactuallow.begin(),
                            ::tolower
                        );

                        if (nombreactuallow.find(nombrebuscadolow) != string::npos) {
                            cout << "**** EL NOMBRE SE ENCONTRO ****" << endl;
                            cout << "NOMBRE: " << gestion[i].nombre << endl;
                            cout << "EDAD: " << gestion[i].edad << endl;

                            for (int j = 0; j < gestion[i].calificaciones.size(); j++) {
                                cout << "NOTA " << j + 1 << ": " << gestion[i].calificaciones[j] << endl;
                            }

                            cout << "----------------------------------------" << endl;
                            encontrado = true;
                        }
                    }

                    if (!encontrado) {
                        cout << "NO SE ENCONTRO EL ESTUDIANTE." << endl;
                    }
                }

                break;
            }

            case 4: {
                cout << "*********** ESTUDIANTE(S) CON MEJOR PROMEDIO ***************" << endl;

                if (cantidad == 0) {
                    cout << "TODAVÍA NO SE HAN INGRESADO ESTUDIANTES" << endl;
                } else {
                    float mejorPromedio = 0.0;
                    vector<int> indicesMejorPromedio;

                    for (int i = 0; i < cantidad; i++) {
                        float suma = 0;

                        for (int j = 0; j < gestion[i].calificaciones.size(); j++) {
                            suma += gestion[i].calificaciones[j];
                        }

                        float promedio = suma / gestion[i].calificaciones.size();

                        if (promedio > mejorPromedio) {
                            mejorPromedio = promedio;
                            indicesMejorPromedio.clear();
                            indicesMejorPromedio.push_back(i); // PUSH BACK HACE QUE LEEA TODO EL VERCOR 
                        } else if (promedio == mejorPromedio) {
                            indicesMejorPromedio.push_back(i);
                        }
                    }

                    cout << fixed << setprecision(2);
                    cout << "MEJOR PROMEDIO: " << mejorPromedio << endl << endl;

                    for (int i : indicesMejorPromedio) {
                        cout << "NOMBRE: " << gestion[i].nombre << endl;
                        cout << "EDAD: " << gestion[i].edad << endl;
                        cout << "NOTAS: ";
                        for (int nota : gestion[i].calificaciones) {
                            cout << nota << " ";
                        }
                        cout << endl;
                        cout << "----------------------------------------" << endl;
                    }
                }

                break;
            }

            case 5: {
                cout << "*********** CREAR ARCHIVO DE ESTUDIANTES APROBADOS ***************" << endl;

                if (cantidad == 0) {
                    cout << "❌ No hay estudiantes ingresados todavía." << endl;
                } else {
                    bool hayAprobados = false;
                    ofstream archivo("aprobados.txt", ios::app);

                    if (!archivo.is_open()) {
                        cout << "❌ No se pudo crear el archivo." << endl;
                        break;
                    }

                    archivo << "LISTA DE ESTUDIANTES APROBADOS:\n\n";

                    for (int i = 0; i < cantidad; i++) {
                        float suma = 0;
                        for (int nota : gestion[i].calificaciones) {
                            suma += nota;
                        }

                        float promedio = suma / gestion[i].calificaciones.size();

                        if (promedio >= 6.0) { // Ahora aprobado es promedio >= 6.0
                            hayAprobados = true;

                            archivo << "Nombre: " << gestion[i].nombre << endl;
                            archivo << "Edad: " << gestion[i].edad << endl;
                            archivo << "Promedio: " << fixed << setprecision(2) << promedio << endl;
                            archivo << "Notas: ";
                            for (int nota : gestion[i].calificaciones) {
                                archivo << nota << " ";
                            }
                            archivo << "\n---------------------------\n";
                        }
                    }

                    archivo.close();

                    if (hayAprobados) {
                        cout << "✅ Archivo 'aprobados.txt' creado correctamente.\n";
                    } else {
                        cout << "⚠️ No hay estudiantes aprobados. El archivo se creó vacío.\n";
                    }
                }

                break;
            }

            case 6: {
                cout << "*********** ESTADÍSTICAS GENERALES ***************" << endl;

                if (cantidad == 0) {
                    cout << "❌ No hay estudiantes ingresados para mostrar estadísticas." << endl;
                } else {
                    int aprobados = 0;
                    int reprobados = 0;
                    float sumaGeneral = 0;
                    float promedioGeneral = 0;
                    float promedioMayor = 0;
                    float promedioMenor = 10;  // Ahora el máximo es 10

                    for (int i = 0; i < cantidad; i++) {
                        float suma = 0;
                        for (int nota : gestion[i].calificaciones) {
                            suma += nota;
                        }

                        float promedio = suma / gestion[i].calificaciones.size();
                        sumaGeneral += promedio;

                        if (promedio >= 6.0) {
                            aprobados++;
                        } else {
                            reprobados++;
                        }

                        if (promedio > promedioMayor) {
                            promedioMayor = promedio;
                        }

                        if (promedio < promedioMenor) {
                            promedioMenor = promedio;
                        }
                    }

                    promedioGeneral = sumaGeneral / cantidad;

                    cout << fixed << setprecision(2);
                    cout << "👥 Total de estudiantes: " << cantidad << endl;
                    cout << "📊 Promedio general: " << promedioGeneral << endl;
                    cout << "✅ Estudiantes aprobados: " << aprobados << endl;
                    cout << "❌ Estudiantes reprobados: " << reprobados << endl;
                    cout << "🔝 Promedio más alto: " << promedioMayor << endl;
                    cout << "🔻 Promedio más bajo: " << promedioMenor << endl;
                }

                break;
            }

            case 0: {
                cout << "************ FIN DEL PROGRAMA EXCITOSO *************" << endl;
                break;
            }

            default:
                cout << "OPCIÓN INVÁLIDA" << endl;
                break;
        }

    } while (opciones != 0);

    return 0;
}
