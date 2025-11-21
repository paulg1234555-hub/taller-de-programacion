# taller-de-programacion
#include <stdio.h>
#include <string.h>

#define MAX 10
#define LEN 30

int main() {
    char nombres[MAX][LEN];
    int cant[MAX], tiempo[MAX], rec[MAX];
    int n = 0;  
    int opcion;

    do {
        printf("\n--- MENU ---\n");
        printf("1. Ingresar producto\n");
        printf("2. Editar producto\n");
        printf("3. Eliminar producto\n");
        printf("4. Mostrar productos\n");
        printf("5. Calcular totales\n");
        printf("6. Verificar demanda\n");
        printf("0. Salir\n");
        printf("Opcion: ");
        scanf("%d", &opcion);

        // INGRESAR
        if (opcion == 1) {
            if (n >= MAX) {
                printf("No se pueden agregar mas productos.\n");
                continue;
            }
            printf("Nombre: ");
            scanf(" %[^\n]", nombres[n]);
            printf("Cantidad: ");
            scanf("%d", &cant[n]);
            printf("Tiempo por unidad: ");
            scanf("%d", &tiempo[n]);
            printf("Recursos por unidad: ");
            scanf("%d", &rec[n]);

            n++;
            printf("Producto agregado.\n");
        }

        // EDITAR
        else if (opcion == 2) {
            char buscar[LEN];
            printf("Nombre a editar: ");
            scanf(" %[^\n]", buscar);

            int pos = -1;
            for (int i = 0; i < n; i++) {
                if (strcmp(nombres[i], buscar) == 0) {
                    pos = i;
                    break;
                }
            }

            if (pos == -1) {
                printf("Producto no encontrado.\n");
            } else {
                printf("Nuevo nombre: ");
                scanf(" %[^\n]", nombres[pos]);
                printf("Nueva cantidad: ");
                scanf("%d", &cant[pos]);
                printf("Nuevo tiempo: ");
                scanf("%d", &tiempo[pos]);
                printf("Nuevos recursos: ");
                scanf("%d", &rec[pos]);
                printf("Producto actualizado.\n");
            }
        }

        // ELIMINAR
        else if (opcion == 3) {
            char buscar[LEN];
            printf("Nombre a eliminar: ");
            scanf(" %[^\n]", buscar);

            int pos = -1;
            for (int i = 0; i < n; i++) {
                if (strcmp(nombres[i], buscar) == 0) {
                    pos = i;
                    break;
                }
            }

            if (pos == -1) {
                printf("Producto no encontrado.\n");
            } else {
                for (int i = pos; i < n - 1; i++) {
                    strcpy(nombres[i], nombres[i + 1]);
                    cant[i] = cant[i + 1];
                    tiempo[i] = tiempo[i + 1];
                    rec[i] = rec[i + 1];
                }
                n--;
                printf("Producto eliminado.\n");
            }
        }

        // MOSTRAR
        else if (opcion == 4) {
            if (n == 0)
                printf("No hay productos.\n");
            else {
                for (int i = 0; i < n; i++)
                    printf("%d) %s | %d uds | t=%d | r=%d\n",
                           i + 1, nombres[i], cant[i], tiempo[i], rec[i]);
            }
        }

        // TOTALES
        else if (opcion == 5) {
            int totalT = 0, totalR = 0;
            for (int i = 0; i < n; i++) {
                totalT += cant[i] * tiempo[i];
                totalR += cant[i] * rec[i];
            }
            printf("Tiempo total requerido: %d\n", totalT);
            printf("Recursos totales requeridos: %d\n", totalR);
        }

        // VERIFICAR DEMANDA
        else if (opcion == 6) {
            int totalT = 0, totalR = 0;
            for (int i = 0; i < n; i++) {
                totalT += cant[i] * tiempo[i];
                totalR += cant[i] * rec[i];
            }

            int dispT, dispR;
            printf("Tiempo disponible: ");
            scanf("%d", &dispT);
            printf("Recursos disponibles: ");
            scanf("%d", &dispR);

            if (totalT <= dispT && totalR <= dispR)
                printf("SI se puede cumplir la demanda.\n");
            else
                printf("NO se puede cumplir la demanda.\n");
        }

    } while (opcion != 0);

    return 0;
}
