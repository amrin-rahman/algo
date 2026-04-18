# algo
``` c
#include <stdio.h>
#include <limits.h>
#include <stdbool.h>

#define MAX 20

int n;
int graph[MAX][MAX];
int vehicle[MAX];

// Function to find minimum distance node
int minDistance(int dist[], bool visited[]) {
    int min = INT_MAX, min_index = -1;

    for (int v = 0; v < n; v++) {
        if (!visited[v] && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }
    return min_index;
}

// Function to print shortest path
void printPath(int parent[], int j) {
    if (parent[j] == -1) {
        printf("%d ", j);
        return;
    }

    printPath(parent, parent[j]);
    printf("%d ", j);
}

// Dijkstra Algorithm
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

            if (!visited[v] &&
                graph[u][v] &&
                dist[u] != INT_MAX &&
                dist[u] + graph[u][v] < dist[v]) {

                parent[v] = u;
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }

    printf("\nShortest Distance: %d\n", dist[dest]);

    printf("Shortest Path: ");
    printPath(parent, dest);
    printf("\n");
}

// Greedy Signal Optimization
void signalOptimization() {

    int maxVehicle = -1;
    int lane = -1;

    for (int i = 0; i < n; i++) {

        if (vehicle[i] > maxVehicle) {
            maxVehicle = vehicle[i];
            lane = i;
        }
    }

    printf("\nTraffic Signal Optimization\n");
    printf("Lane %d gets GREEN signal ", lane);
    printf("(Vehicles: %d)\n", maxVehicle);
}

// Dynamic Programming Traffic Analysis
void trafficAnalysis() {

    int dp[MAX];

    dp[0] = vehicle[0];

    for (int i = 1; i < n; i++) {
        dp[i] = dp[i - 1] + vehicle[i];
    }

    int maxTraffic = vehicle[0];
    int peakLane = 0;

    for (int i = 1; i < n; i++) {

        if (vehicle[i] > maxTraffic) {
            maxTraffic = vehicle[i];
            peakLane = i;
        }
    }

    printf("\nTraffic Pattern Analysis\n");
    printf("Peak Traffic Lane: %d\n", peakLane);
    printf("Maximum Vehicles: %d\n", maxTraffic);
}

// Congestion Detection
void congestionCheck() {

    int threshold = 50;

    printf("\nCongestion Status:\n");

    for (int i = 0; i < n; i++) {

        if (vehicle[i] > threshold) {

            printf("Lane %d is CONGESTED\n", i);
        }
        else {

            printf("Lane %d Normal\n", i);
        }
    }
}

int main() {

    int edges;

    printf("Smart Traffic Management System\n");

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter number of edges: ");
    scanf("%d", &edges);

    // Initialize Graph
    for (int i = 0; i < n; i++) {

        for (int j = 0; j < n; j++) {

            graph[i][j] = 0;
        }
    }

    printf("Enter edges (u v weight):\n");

    for (int i = 0; i < edges; i++) {

        int u, v, w;

        scanf("%d %d %d", &u, &v, &w);

        graph[u][v] = w;
        graph[v][u] = w;
    }

    printf("\nEnter vehicle count for each lane:\n");

    for (int i = 0; i < n; i++) {

        scanf("%d", &vehicle[i]);
    }

    // Greedy Algorithm
    signalOptimization();

    // Dynamic Programming
    trafficAnalysis();

    // Congestion Check
    congestionCheck();

    int src, dest;

    printf("\nEnter source node: ");
    scanf("%d", &src);

    printf("Enter destination node: ");
    scanf("%d", &dest);

    // Dijkstra Algorithm
    dijkstra(src, dest);

    printf("\nSystem Execution Completed\n");

    return 0;
}
```



```c
2.
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
