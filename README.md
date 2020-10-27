# TrabajoIntegradorRC
Trabajo de aplicación Bellman-Ford

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
#include <bits/stdc++.h>

struct Arista
{
    int aristainicio, aristallegada, peso;
};

struct Grafico {
    int numvertice, numarista;
    struct Arista* Arista;
};

struct Grafico* CrearGrafico(int numvertice, int numarista)
{
    struct Grafico* grafico = new Grafico;
    grafico->numvertice = numvertice;
    grafico->numarista = numarista;
    grafico->Arista = new Arista[numarista];
    return grafico;
}

void ImprimirV(int dist[], int n)
{
    printf("\nVertice\tDistancia desde fuente de Vertice\n");
    int i;
    for (i = 0; i < n; ++i){
		printf("%d \t\t %d\n", i, dist[i]);
	}
}
void BellmanFord(struct Grafico* grafico, int fuente)
{
    int numvertice = grafico->numvertice;
    int numarista = grafico->numarista;
    int almacen[numvertice];

    int i,j;
    for (i = 0; i < numvertice; i++)
        almacen[i] = 1000;
    almacen[fuente] = 0;

    for (i = 1; i <= numvertice-1; i++)
    {
        for (j = 0; j < numarista; j++)
        {
            int u = grafico->Arista[j].aristainicio;
            int v = grafico->Arista[j].aristallegada;
            int peso = grafico->Arista[j].peso;
            if (almacen[u] + peso < almacen[v])
                almacen[v] = almacen[u] + peso;
        }
    }

    for (i = 0; i < numarista; i++)
    {
        int u = grafico->Arista[i].aristainicio;
        int v = grafico->Arista[i].aristallegada;
        int peso = grafico->Arista[i].peso;
        if (almacen[u] + peso < almacen[v])
            printf("Este gráfico contiene un ciclo de arista negativo\n");
    }

    ImprimirV(almacen, numvertice);
    return;
}

int main()
{
    int numvertice,numarista,fuente;
	printf("Ingresa el numero de vertices que quiere en el grafico:\n");
    scanf("%d",&numvertice);

	printf("Ingresa el numero de aristas que quiere en el grafico:\n");
    scanf("%d",&numarista);

	printf("Ingresa el numero de fuente vertice:\n");
	scanf("%d",&fuente);

    struct Grafico* grafico = CrearGrafico(numvertice, numarista);

    int i;
    for(i=0;i<numarista;i++){
        printf("\nIngresar las propiedades de la arista %d: Inicio, Fin y Peso respectivamente\n",i+1);
        scanf("%d",&grafico->Arista[i].aristainicio);
        scanf("%d",&grafico->Arista[i].aristallegada);
        scanf("%d",&grafico->Arista[i].peso);
    }
    BellmanFord(grafico, fuente);
    return 0;
}
