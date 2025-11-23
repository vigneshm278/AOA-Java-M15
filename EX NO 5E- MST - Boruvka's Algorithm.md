
# EX 5E Minimum Spanning Tree -Boruvka's Algorithm
## AIM:
To write a Java program to for given constraints.
Boruvka's Algorithm - Minimum Spanning Tree

Find the MST using Boruvka's Algorithm for a weighted undirected graph.
<img width="292" height="235" alt="image" src="https://github.com/user-attachments/assets/06246b27-37a9-40a8-bd7a-37a1d5187cd1" />

## Algorithm

1. **Input:**

   * Read number of vertices `V` and edges `E`.
   * For each edge, read its source `src`, destination `dest`, and weight `w`.
   * Store all edges in a list.

2. **Initialization:**

   * Set up a `parent[]` array for Disjoint Set Union (DSU).
   * Initially, each vertex is its own parent.
   * Initialize `numTrees = V` (number of connected components) and `MSTweight = 0`.

3. **Finding Cheapest Edges:**

   * For every iteration (while more than one tree exists):

     * Initialize an array `cheapest[]` to store the minimum-cost edge for each component.
     * For each edge, find the sets of its two vertices.
     * Update `cheapest` for both sets if the current edge has a smaller weight.

4. **Building the MST:**

   * For each vertex, pick its `cheapest` edge (if any).
   * If the two endpoints belong to different sets, include the edge in the MST, print it, and merge the sets using `union()`.
   * Decrease `numTrees` after each successful merge.

5. **Output:**

   * Continue until only one tree (MST) remains.
   * Print each chosen edge and finally display the **Total Weight of MST**.
   

## Program:
```
/*
Program to implement Reverse a String
Developed by: Vignesh M
Register Number: 212223240176
*/
import java.util.*;

public class BoruvkaMST {
    static int[] parent;

    static int find(int i) {
        if (parent[i] != i)
            parent[i] = find(parent[i]);
        return parent[i];
    }

    static void union(int x, int y) {
        parent[find(x)] = find(y);
    }

    static int boruvkaMST(int V, List<Edge> edges) {
        parent = new int[V];
        int[] cheapest = new int[V];
        int numTrees = V;
        int MSTweight = 0;

        for (int v = 0; v < V; v++)
            parent[v] = v;

        while (numTrees > 1) {
            Arrays.fill(cheapest, -1);

            for (int i = 0; i < edges.size(); i++) {
                int set1 = find(edges.get(i).src);
                int set2 = find(edges.get(i).dest);
                if (set1 == set2) continue;

                if (cheapest[set1] == -1 || edges.get(i).weight < edges.get(cheapest[set1]).weight)
                    cheapest[set1] = i;

                if (cheapest[set2] == -1 || edges.get(i).weight < edges.get(cheapest[set2]).weight)
                    cheapest[set2] = i;
            }

            for (int i = 0; i < V; i++) {
                if (cheapest[i] != -1) {
                    Edge e = edges.get(cheapest[i]);
                    int set1 = find(e.src);
                    int set2 = find(e.dest);

                    if (set1 == set2) continue;

                    MSTweight += e.weight;
                    System.out.println("Edge: " + e.src + "-" + e.dest + " Weight: " + e.weight);
                    union(set1, set2);
                    numTrees--;
                }
            }
        }
        return MSTweight;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int V = sc.nextInt();
        int E = sc.nextInt();

        List<Edge> edges = new ArrayList<>();
        for (int i = 0; i < E; i++) {
            edges.add(new Edge(sc.nextInt(), sc.nextInt(), sc.nextInt()));
        }

        int totalWeight = boruvkaMST(V, edges);
        System.out.println("Total Weight of MST: " + totalWeight);

        sc.close();
    }
}

class Edge {
    int src, dest, weight;
    Edge(int s, int d, int w) {
        src = s; dest = d; weight = w;
    }
}

```

## Output:
<img width="736" height="485" alt="image" src="https://github.com/user-attachments/assets/7e88ce2d-fabe-455f-8fc9-8ec6622ab5e3" />



## Result:
The program successfully implemented and the expected output is verified.
