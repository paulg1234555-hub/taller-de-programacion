Estructura de archivos

/proyecto
│── main.c
│── zona.h
│── zona.c
│── calculos.h
│── calculos.c
│── reporte.h
│── reporte.c

Zona.h
#ifndef ZONA_H
#define ZONA_H

#define ZONAS 5
#define DIAS 30

// Límites OMS (referenciales)
#define LIM_CO2 400
#define LIM_NO2 200
#define LIM_PM25 25

struct Zona {
    char nombre[30];
    float co2[DIAS];
    float no2[DIAS];
    float pm25[DIAS];
};

void ingresarDatos(struct Zona zonas[]);
void mostrarResultados(struct Zona zonas[]);

#endif

Cálculos.h
#ifndef CALCULOS_H
#define CALCULOS_H

float promedio(float datos[]);
float prediccion(float datos[]);

#endif

Reporte.h
#ifndef REPORTE_H
#define REPORTE_H

#include "zona.h"

void guardarReporte(struct Zona zonas[]);

#endif

Zona.c
#include <stdio.h>
#include "zona.h"
#include "calculos.h"

void ingresarDatos(struct Zona zonas[]) {
    for (int i = 0; i < ZONAS; i++) {
        printf("\n--- Ingreso de datos para %s ---\n", zonas[i].nombre);
        for (int d = 0; d < DIAS; d++) {
            printf("Dia %d - Dioxido de Carbono (CO2): ", d + 1);
            scanf("%f", &zonas[i].co2[d]);

            printf("Dia %d - Dioxido de Nitrogeno (NO2): ", d + 1);
            scanf("%f", &zonas[i].no2[d]);

            printf("Dia %d - Material Particulado PM2.5: ", d + 1);
            scanf("%f", &zonas[i].pm25[d]);
        }
    }
}

void mostrarResultados(struct Zona zonas[]) {
    for (int i = 0; i < ZONAS; i++) {
        float promCO2 = promedio(zonas[i].co2);
        float promNO2 = promedio(zonas[i].no2);
        float promPM  = promedio(zonas[i].pm25);

        float predCO2 = prediccion(zonas[i].co2);
        float predNO2 = prediccion(zonas[i].no2);
        float predPM  = prediccion(zonas[i].pm25);

        printf("\n===== %s =====\n", zonas[i].nombre);

        printf("Promedio Dioxido de Carbono (CO2): %.2f\n", promCO2);
        printf("Promedio Dioxido de Nitrogeno (NO2): %.2f\n", promNO2);
        printf("Promedio Material Particulado PM2.5: %.2f\n", promPM);

        printf("Prediccion Dioxido de Carbono (CO2): %.2f\n", predCO2);
        printf("Prediccion Dioxido de Nitrogeno (NO2): %.2f\n", predNO2);
        printf("Prediccion Material Particulado PM2.5: %.2f\n", predPM);

        if (predCO2 > LIM_CO2 || predNO2 > LIM_NO2 || predPM > LIM_PM25) {
            printf("⚠ ALERTA: Niveles de contaminacion elevados\n");
            printf("- Reducir trafico vehicular\n");
            printf("- Evitar actividades al aire libre\n");
            printf("- Controlar emisiones industriales\n");
        } else {
            printf("Niveles de contaminacion aceptables.\n");
        }
    }
}

Calculos.c
#include "calculos.h"
#include "zona.h"

float promedio(float datos[]) {
    float suma = 0;
    for (int i = 0; i < DIAS; i++) {
        suma += datos[i];
    }
    return suma / DIAS;
}

float prediccion(float datos[]) {
    float suma = 0, pesoTotal = 0;
    int peso = 1;

    for (int i = 0; i < DIAS; i++) {
        suma += datos[i] * peso;
        pesoTotal += peso;
        peso++;
    }
    return suma / pesoTotal;
}

Reporte.c
#include <stdio.h>
#include "reporte.h"
#include "calculos.h"

void guardarReporte(struct Zona zonas[]) {
    FILE *archivo = fopen("reporte_contaminacion.txt", "w");

    if (archivo == NULL) {
        printf("Error al crear el archivo.\n");
        return;
    }

    for (int i = 0; i < ZONAS; i++) {
        fprintf(archivo, "Zona: %s\n", zonas[i].nombre);
        fprintf(archivo, "Promedio CO2: %.2f\n", promedio(zonas[i].co2));
        fprintf(archivo, "Promedio NO2: %.2f\n", promedio(zonas[i].no2));
        fprintf(archivo, "Promedio PM2.5: %.2f\n", promedio(zonas[i].pm25));
        fprintf(archivo, "Prediccion CO2: %.2f\n", prediccion(zonas[i].co2));
        fprintf(archivo, "Prediccion NO2: %.2f\n", prediccion(zonas[i].no2));
        fprintf(archivo, "Prediccion PM2.5: %.2f\n\n", prediccion(zonas[i].pm25));
    }

    fclose(archivo);
}

Main.c
#include <stdio.h>
#include "zona.h"
#include "reporte.h"

int main() {

    struct Zona zonas[ZONAS] = {
        {"Zona Norte"},
        {"Zona Sur"},
        {"Zona Este"},
        {"Zona Oeste"},
        {"Zona Centro"}
    };

    ingresarDatos(zonas);
    mostrarResultados(zonas);
    guardarReporte(zonas);

    printf("\nPrograma finalizado correctamente.\n");
    return 0;
}
