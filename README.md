# taller-de-programacion
trabajo de progra


Archivo: funciones.h

#ifndef FUNCIONES_H
#define FUNCIONES_H

#define MAX 10
#define MAX_NOMBRE 30

void ingresarProductos(char nombres[][MAX_NOMBRE], float precios[], int n);
void mostrarProductos(char nombres[][MAX_NOMBRE], float precios[], int n);
float calcularTotal(float precios[], int n);
float calcularPromedio(float precios[], int n);
void productoMasCaro(char nombres[][MAX_NOMBRE], float precios[], int n);
void productoMasBarato(char nombres[][MAX_NOMBRE], float precios[], int n);
void buscarProducto(char nombres[][MAX_NOMBRE], float precios[], int n, char buscado[]);

#endif

Archivo: funciones.c

#include <stdio.h>
#include <string.h>
#include "funciones.h"

void ingresarProductos(char nombres[][MAX_NOMBRE], float precios[], int n) {
    for (int i = 0; i < n; i++) {
        printf("\nProducto %d\n", i + 1);
        printf("Nombre: ");
        scanf(" %[^
]", nombres[i]);
        printf("Precio: ");
        scanf("%f", &precios[i]);
    }
}
...

Archivo: main.c

#include <stdio.h>
#include <string.h>
#include "funciones.h"

int main() {
    int n;
    char nombres[MAX][MAX_NOMBRE];
    float precios[MAX];
    char buscado[MAX_NOMBRE];

    printf("¿Cuántos productos desea ingresar (máximo 10)? ");
    scanf("%d", &n);

    ingresarProductos(nombres, precios, n);
    mostrarProductos(nombres, precios, n);

    printf("\nTotal del inventario: $%.2f\n", calcularTotal(precios, n));
    printf("Promedio de precios: $%.2f\n", calcularPromedio(precios, n));
    productoMasCaro(nombres, precios, n);
    productoMasBarato(nombres, precios, n);

    printf("\nIngrese el nombre del producto que desea buscar: ");
    scanf(" %[^
]", buscado);
    buscarProducto(nombres, precios, n, buscado);

    return 0;
}

