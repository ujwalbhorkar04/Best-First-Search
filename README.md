# Best-First-Search
Implementation of Best-First Search using C language 

#include <stdio.h>
#include <stdlib.h>

#define MAX 100
#define INF 99999 // Infinite cost for unreachable nodes

int n; // Number of nodes
int adjMatrix[MAX][MAX]; // Graph adjacency matrix
int visited[MAX] = {0}; // Marks visited nodes
int heuristic[MAX]; // Heuristic values (cost estimates)


typedef struct {
    int node;
    int cost;
} PQNode;

PQNode pq[MAX]; // Priority queue
int pqSize = 0;


void swap(int i, int j) {
    PQNode temp = pq[i];
    pq[i] = pq[j];
    pq[j] = temp;
}


void enqueue(int node, int cost) {
    pq[pqSize].node = node;
    pq[pqSize].cost = cost;
    pqSize++;

    
    for (int i = pqSize - 1; i > 0 && pq[i].cost < pq[i - 1].cost; i--) {
        swap(i, i - 1);
    }
}


int dequeue() {
    if (pqSize == 0) return -1;
    int node = pq[0].node;
    for (int i = 0; i < pqSize - 1; i++) {
        pq[i] = pq[i + 1];
    }
    pqSize--;
    return node;
}


void bestFirstSearch(int startNode, int goalNode) {
    enqueue(startNode, heuristic[startNode]);
    visited[startNode] = 1;

    while (pqSize > 0) {
        int currentNode = dequeue();
        printf("Visited: %d\n", currentNode);

        if (currentNode == goalNode) {
            printf("Reached goal node: %d\n", goalNode);
            return;
        }

 
        for (int i = 0; i < n; i++) {
            if (adjMatrix[currentNode][i] && !visited[i]) {
                enqueue(i, heuristic[i]);
                visited[i] = 1;
            }
        }
    }
}

int main() {
    int edges, u, v, start, goal, weight;

    printf("Enter the number of nodes: ");
    scanf("%d", &n);

    printf("Enter the number of edges: ");
    scanf("%d", &edges);

   
    for (int i = 0; i < edges; i++) {
        printf("Enter edge (u v) and weight: ");
        scanf("%d %d %d", &u, &v, &weight);
        adjMatrix[u][v] = weight;
        adjMatrix[v][u] = weight; // For undirected graph
    }

    // Input heuristic values (cost estimates for each node)
    printf("Enter the heuristic values for each node:\n");
    for (int i = 0; i < n; i++) {
        printf("Heuristic of node %d: ", i);
        scanf("%d", &heuristic[i]);
    }

    printf("Enter the start node: ");
    scanf("%d", &start);

    printf("Enter the goal node: ");
    scanf("%d", &goal);

    bestFirstSearch(start, goal);

    return 0;
}

