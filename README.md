estructuras.h

#ifndef ESTRUCTURAS_H
#define ESTRUCTURAS_H

typedef struct {
    int id;
    char marca[20];
    char modelo[20];
    char tipo[15];
    int anio;
    float precio;
    char estado[10];
    int disponible;
} Vehiculo;

typedef struct {
    int id;
    char nombre[40];
    int edad;
    float presupuesto;
} Cliente;

typedef struct {
    int idVenta;
    int idCliente;
    int idVehiculo;
    float precioVenta;
} Venta;

#endif


funciones.h
#ifndef FUNCIONES_H
#define FUNCIONES_H

void menu();
void registrarVehiculo();
void listarVehiculos();
void registrarCliente();
void buscarVehiculos(float presupuesto);
void registrarVenta(int idCliente, int idVehiculo, float precio);

#endif


funciones.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "estructuras.h"
#include "funciones.h"

#define VEHICULOS_FILE "vehiculos.dat"
#define CLIENTES_FILE  "clientes.dat"
#define VENTAS_FILE    "ventas.dat"

// ---------------- MENU ----------------
void menu() {
    int opcion;
    do {
        printf("\n===== RUEDAS DE ORO =====\n");
        printf("1. Registrar vehiculo\n");
        printf("2. Listar vehiculos\n");
        printf("3. Registrar cliente\n");
        printf("4. Buscar vehiculos (Caso Ruben)\n");
        printf("0. Salir\n");
        printf("Opcion: ");
        scanf("%d", &opcion);

        switch (opcion) {
            case 1: registrarVehiculo(); break;
            case 2: listarVehiculos(); break;
            case 3: registrarCliente(); break;
            case 4: buscarVehiculos(14000); break;
            case 0: printf("Saliendo...\n"); break;
            default: printf("Opcion invalida\n");
        }
    } while (opcion != 0);
}

// ---------------- VEHICULOS ----------------
void registrarVehiculo() {
    FILE *f = fopen(VEHICULOS_FILE, "ab");
    Vehiculo v;

    if (!f) return;

    printf("ID: ");
    scanf("%d", &v.id);
    printf("Marca: ");
    scanf("%s", v.marca);
    printf("Modelo: ");
    scanf("%s", v.modelo);
    printf("Tipo: ");
    scanf("%s", v.tipo);
    printf("Anio: ");
    scanf("%d", &v.anio);
    printf("Precio: ");
    scanf("%f", &v.precio);
    printf("Estado (nuevo/usado): ");
    scanf("%s", v.estado);

    v.disponible = 1;
    fwrite(&v, sizeof(Vehiculo), 1, f);
    fclose(f);

    printf("Vehiculo registrado.\n");
}

void listarVehiculos() {
    FILE *f = fopen(VEHICULOS_FILE, "rb");
    Vehiculo v;

    if (!f) return;

    while (fread(&v, sizeof(Vehiculo), 1, f)) {
        if (v.disponible) {
            printf("ID:%d | %s %s | %s | $%.2f\n",
                   v.id, v.marca, v.modelo, v.tipo, v.precio);
        }
    }
    fclose(f);
}

// ---------------- CLIENTES ----------------
void registrarCliente() {
    FILE *f = fopen(CLIENTES_FILE, "ab");
    Cliente c;

    if (!f) return;

    printf("ID: ");
    scanf("%d", &c.id);
    printf("Nombre: ");
    scanf(" %[^\n]", c.nombre);
    printf("Edad: ");
    scanf("%d", &c.edad);
    printf("Presupuesto: ");
    scanf("%f", &c.presupuesto);

    fwrite(&c, sizeof(Cliente), 1, f);
    fclose(f);

    printf("Cliente registrado.\n");
}

// ---------------- BUSQUEDA Y VENTA ----------------
void buscarVehiculos(float presupuesto) {
    FILE *f = fopen(VEHICULOS_FILE, "rb");
    Vehiculo v;
    int encontrado = 0;

    if (!f) return;

    printf("\nCamionetas Chevrolet usadas <= $%.2f\n", presupuesto);

    while (fread(&v, sizeof(Vehiculo), 1, f)) {
        if (v.disponible &&
            strcmp(v.tipo, "camioneta") == 0 &&
            strcmp(v.estado, "usado") == 0 &&
            strcmp(v.marca, "Chevrolet") == 0 &&
            v.precio <= presupuesto) {

            printf("ID:%d | %s %s | $%.2f\n",
                   v.id, v.marca, v.modelo, v.precio);
            encontrado = 1;
        }
    }
    fclose(f);

    if (!encontrado) {
        printf("No hay coincidencias.\n");
        return;
    }

    int idCliente, idVehiculo;
    float precio;

    printf("ID Cliente: ");
    scanf("%d", &idCliente);
    printf("ID Vehiculo: ");
    scanf("%d", &idVehiculo);
    printf("Precio venta: ");
    scanf("%f", &precio);

    registrarVenta(idCliente, idVehiculo, precio);
}

void registrarVenta(int idCliente, int idVehiculo, float precio) {
    FILE *fv = fopen(VEHICULOS_FILE, "rb+");
    FILE *fven = fopen(VENTAS_FILE, "ab");
    Vehiculo v;
    Venta ven;

    if (!fv || !fven) return;

    while (fread(&v, sizeof(Vehiculo), 1, fv)) {
        if (v.id == idVehiculo && v.disponible) {
            v.disponible = 0;
            fseek(fv, -sizeof(Vehiculo), SEEK_CUR);
            fwrite(&v, sizeof(Vehiculo), 1, fv);
            break;
        }
    }

    ven.idVenta = rand() % 10000;
    ven.idCliente = idCliente;
    ven.idVehiculo = idVehiculo;
    ven.precioVenta = precio;

    fwrite(&ven, sizeof(Venta), 1, fven);

    fclose(fv);
    fclose(fven);

    printf("Venta registrada con exito.\n");
}
