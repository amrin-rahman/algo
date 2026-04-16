# algo
``` c
#include <stdio.h>
#include <limits.h>
#include <stdbool.h>

#define MAX 100

int n; 
int graph[MAX][MAX];
int vehicle[MAX];

int minDistance(int dist[], bool visited[]) {
    int min = INT_MAX, min_index;

    for (int v = 0; v < n; v++) {
        if (!visited[v] && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }
    return min_index;
}

void dijkstra(int src, int dest) {
    int dist[MAX];
    bool visited[MAX];
    int parent[MAX];

    for (int i = 0; i < n; i++) {
        dist[i] = INT_MAX;
        visited[i] = false;
        parent[i] = -1;
    }

    dist[src] = 0;

    for (int count = 0; count < n - 1; count++) {
        int u = minDistance(dist, visited);
        visited[u] = true;

        for (int v = 0; v < n; v++) {
            if (!visited[v] && graph[u][v] &&
                dist[u] != INT_MAX &&
                dist[u] + graph[u][v] < dist[v]) {

                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }

    printf("\nShortest distance from %d to %d = %d\n", src, dest, dist[dest]);

    printf("Path: ");
    int path[MAX], count = 0;
    int current = dest;

    while (current != -1) {
        path[count++] = current;
        current = parent[current];
    }

    for (int i = count - 1; i >= 0; i--) {
        printf("%d ", path[i]);
    }
    printf("\n");
}

void signalOptimization() {
    int maxVehicle = -1, lane = -1;

    for (int i = 0; i < n; i++) {
        if (vehicle[i] > maxVehicle) {
            maxVehicle = vehicle[i];
            lane = i;
        }
    }

    printf("\nTraffic Signal Optimization:\n");
    printf("Lane %d gets GREEN signal (Max vehicles: %d)\n", lane, maxVehicle);
}

int main() {
    int edges;

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter number of edges: ");
    scanf("%d", &edges);

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            graph[i][j] = 0;

    printf("Enter edges (u v weight):\n");
    for (int i = 0; i < edges; i++) {
        int u, v, w;
        scanf("%d %d %d", &u, &v, &w);
        graph[u][v] = w;
        graph[v][u] = w;
    }

    printf("\nEnter vehicle count for each node:\n");
    for (int i = 0; i < n; i++) {
        scanf("%d", &vehicle[i]);
    }

    signalOptimization();

    int src, dest;
    printf("\nEnter source and destination: ");
    scanf("%d %d", &src, &dest);

    dijkstra(src, dest);

    return 0;
}
```
