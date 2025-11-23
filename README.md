
# EX 5A 0/1 Knapsack Problem - Branch&Bound 
## AIM:
To Write a Java program to solve 0/1 Knapsack problem using Branch and Bound Approach.
You are heading a college entrepreneurship cell that can invest in up to N student‑startups.

For each startup i you know: cost[i]  — the amount (in ₹ lakh) required to join the showcase profit[i] — the estimated profit (in ₹ lakh) you’ll gain if it succeeds You have a total budget of B ₹ lakh. Pick a subset of startups so that the sum of costs ≤ B and the sum of profits is maximised.

Because N can be as large as 50, a plain exhaustive search (2^N) is too slow.

The recommended approach is Branch & Bound with a fractional‑knapsack upper bound (but any algorithm that meets the constraints is accepted). 

Input Format

N

B

cost[1] cost[2] … cost[N]

profit[1] profit[2] … profit[N]

1 ≤ N ≤ 50

1 ≤ B ≤ 1 000 000

1 ≤ cost[i], profit[i] ≤ 10 000 

Output Format

maxProfit

For example:




## Algorithm

1. **Input:**

   * Read number of items `N` and bag capacity `B`.
   * Read the `cost` and `profit` of each item.

2. **Initialization:**

   * Create `Item` objects storing `cost`, `profit`, and `profit-to-cost ratio`.
   * Sort items in **descending order of ratio** (profit per cost).

3. **Bounding Function:**

   * Define a `bound()` function to calculate the **upper bound of profit** that can be achieved from a given node using fractional knapsack logic.

4. **Branch and Bound Logic:**

   * Use a **queue** to explore possible item selections.
   * For each node (state), generate two branches:

     * **Include** the next item.
     * **Exclude** the next item.
   * Update `maxProfit` if a valid higher profit is found.
   * Add nodes to the queue only if their bound is greater than current `maxProfit`.

5. **Output:**

   * After exploring all possible branches, print the **maximum achievable profit**.


## Program:
```
/*
Program to implement Reverse a String
Developed by: Vignesh M
Register Number: 212223240176
*/
import java.util.*;

class Item {
    int cost, profit;
    double ratio;
    Item(int cost, int profit) {
        this.cost = cost;
        this.profit = profit;
        this.ratio = (double) profit / cost;
    }
}

class Node {
    int level, profit, bound, cost;
    Node(int level, int profit, int cost) {
        this.level = level;
        this.profit = profit;
        this.cost = cost;
    }
}

public class Main {
    static int N, B;
    static Item[] items;

    static int bound(Node u) {
        if (u.cost >= B) return 0;
        int profit_bound = u.profit;
        int j = u.level + 1;
        int totweight = u.cost;

        while (j < N && totweight + items[j].cost <= B) {
            totweight += items[j].cost;
            profit_bound += items[j].profit;
            j++;
        }

        if (j < N)
            profit_bound += (B - totweight) * items[j].ratio;

        return profit_bound;
    }

    static int knapsack() {
        Queue<Node> Q = new LinkedList<>();
        Node u, v;
        u = new Node(-1, 0, 0);
        int maxProfit = 0;
        Q.add(u);

        while (!Q.isEmpty()) {
            u = Q.poll();
            if (u.level == N - 1) continue;

            v = new Node(u.level + 1, u.profit + items[u.level + 1].profit,
                         u.cost + items[u.level + 1].cost);

            if (v.cost <= B && v.profit > maxProfit)
                maxProfit = v.profit;

            v.bound = bound(v);
            if (v.bound > maxProfit)
                Q.add(v);

            v = new Node(u.level + 1, u.profit, u.cost);
            v.bound = bound(v);
            if (v.bound > maxProfit)
                Q.add(v);
        }
        return maxProfit;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        B = sc.nextInt();
        int[] cost = new int[N];
        int[] profit = new int[N];
        for (int i = 0; i < N; i++) cost[i] = sc.nextInt();
        for (int i = 0; i < N; i++) profit[i] = sc.nextInt();

        items = new Item[N];
        for (int i = 0; i < N; i++)
            items[i] = new Item(cost[i], profit[i]);

        Arrays.sort(items, (a, b) -> Double.compare(b.ratio, a.ratio));

        System.out.println(knapsack());
    }
}

```

## Output:
<img width="384" height="228" alt="image" src="https://github.com/user-attachments/assets/b8b50dee-92a7-4b51-ab1b-5bcad37ce2b1" />



## Result:
The program successfully solved 0/1 Knapsack problem using branch & bound and output is verified. 







# EX 5B Topological Sort - Khan's Algorithm
## AIM:
To write a Java program to for given constraints.
Problem Description:
A software development team is preparing for a product release. The release consists of multiple tasks, each dependent on other tasks being completed first. You are to determine a valid order in which all tasks can be completed. If it's not possible due to cyclic dependencies, output that the release cannot be scheduled.

Each task is labeled from 0 to n-1. The dependencies are provided in the form of pairs [a, b] which means task a depends on task b.

Implement a program to find a valid task execution order using topological sort.

Input Format:

An integer n — number of tasks.

An integer m — number of dependencies.

m lines follow with two integers a and b — representing a depends on b.

Output Format:

If a valid order exists, print the task numbers in a possible execution order (space-separated).

If not, print "Release cannot be scheduled".

<img width="341" height="363" alt="image" src="https://github.com/user-attachments/assets/f0355541-4f66-49da-bcd3-171a799a7c1f" />

## Algorithm

1. **Input:**

   * Read the number of tasks `n` and the number of dependency pairs `m`.
   * Read each dependency pair and store them in a 2D array.

2. **Graph Construction:**

   * Create an adjacency list to represent task dependencies.
   * Maintain an `indegree` array to count incoming edges for each task.

3. **Initialization:**

   * Add all tasks with `indegree = 0` (no dependencies) to a queue.

4. **Topological Sorting:**

   * Repeatedly remove a task from the queue, add it to the order list, and decrease the indegree of its dependent tasks.
   * If a dependent task’s indegree becomes 0, add it to the queue.

5. **Output:**

   * If all tasks are processed, print the valid order of execution.
   * Otherwise, display “Release cannot be scheduled” if a cycle (dependency conflict) exists.


## Program:
```
/*
Program to implement Reverse a String
Developed by: Vignesh M
Register Number: 212223240176
*/
import java.util.*;

public class prog {

    public static List<Integer> findTaskOrder(int n, int[][] dependencies) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++)
            adj.add(new ArrayList<>());

        int[] indegree = new int[n];

        for (int[] dep : dependencies) {
            int a = dep[0];
            int b = dep[1];
            adj.get(b).add(a);
            indegree[a]++;
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < n; i++)
            if (indegree[i] == 0)
                q.add(i);

        List<Integer> order = new ArrayList<>();
        while (!q.isEmpty()) {
            int curr = q.poll();
            order.add(curr);
            for (int next : adj.get(curr)) {
                indegree[next]--;
                if (indegree[next] == 0)
                    q.add(next);
            }
        }

        if (order.size() != n)
            return null;
        return order;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();

        int[][] dependencies = new int[m][2];
        for (int i = 0; i < m; i++) {
            dependencies[i][0] = sc.nextInt();
            dependencies[i][1] = sc.nextInt();
        }

        List<Integer> result = findTaskOrder(n, dependencies);

        if (result == null) {
            System.out.println("Release cannot be scheduled");
        } else {
            for (int task : result) {
                System.out.print(task + " ");
            }
        }
    }
}

```

## Output:
<img width="778" height="526" alt="image" src="https://github.com/user-attachments/assets/e974c893-cc88-4e0e-934d-cb727552d82a" />



## Result:
The program successfully implemented and the expected output is verified.




# EX 5C Graph coloring
## AIM:
To write a Java program to for given constraints.
Problem Description:
In a hilly region, several radio towers are installed to provide communication services. However, due to signal interference, two adjacent towers (i.e., in communication range of each other) must not use the same frequency channel.

You are given N radio towers and their communication ranges represented as an undirected graph. Your task is to assign channels (colors) to these towers using at most M channels such that no two adjacent towers use the same channel.

Write a program to determine if such an assignment is possible or not.

Input Format:
First line contains two integers: N (number of towers), and M (number of available frequency channels).

Next line contains an integer E — number of edges representing the communication range.

Next E lines contain two integers u and v — representing that tower u and tower v are within range (0-based index).

Output Format:
Print "YES" if it's possible to assign frequencies to towers such that no two adjacent towers have the same frequency.

Otherwise, print "NO".

<img width="182" height="440" alt="image" src="https://github.com/user-attachments/assets/b32078a2-c79d-4a25-88c4-e51144b5456f" />


## Algorithm

1. **Input:**

   * Read the number of towers `n`, available channels `m`, and number of connections `e`.
   * Read each connection pair `(u, v)` and build an adjacency list for the graph.

2. **Initialization:**

   * Create an integer array `color[n]` initialized with 0 (meaning no channel assigned).
   * Each tower (node) can be assigned a color (channel) from `1` to `m`.

3. **Safety Check (`isSafe`):**

   * Before assigning a channel to a tower, check all its adjacent towers.
   * If any adjacent tower already uses the same channel, assignment is **not safe**.

4. **Backtracking Solution (`solve`):**

   * Try assigning each channel (1 to `m`) to the current tower.
   * If safe, recursively assign channels to the next tower.
   * If no valid channel exists, **backtrack** and try another combination.

5. **Output:**

   * If all towers can be assigned channels without conflict → print `"YES"`.
   * Otherwise → print `"NO"`.
   

## Program:
```
/*
Program to implement Reverse a String
Developed by: Vignesh M
Register Number: 212223240176
*/
import java.util.*;

public class RadioTowerChannelAssignment {

    public static boolean isSafe(List<List<Integer>> graph, int[] color, int node, int c) {
        for (int adj : graph.get(node)) {
            if (color[adj] == c)
                return false;
        }
        return true;
    }

    public static boolean solve(List<List<Integer>> graph, int[] color, int node, int m, int n) {
        if (node == n)
            return true;

        for (int c = 1; c <= m; c++) {
            if (isSafe(graph, color, node, c)) {
                color[node] = c;
                if (solve(graph, color, node + 1, m, n))
                    return true;
                color[node] = 0;
            }
        }
        return false;
    }

    public static boolean isColorable(List<List<Integer>> graph, int[] color, int node, int m, int n) {
        return solve(graph, color, node, m, n);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int e = sc.nextInt();

        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++)
            graph.add(new ArrayList<>());

        for (int i = 0; i < e; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            graph.get(u).add(v);
            graph.get(v).add(u);
        }

        int[] color = new int[n];

        if (isColorable(graph, color, 0, m, n))
            System.out.println("YES");
        else
            System.out.println("NO");

        sc.close();
    }
}

```

## Output:
<img width="520" height="474" alt="image" src="https://github.com/user-attachments/assets/3417cf1d-f702-4ac7-a2dd-1b11369c84b6" />



## Result:
The program successfully implemented and the expected output is verified.




# EX 5D Flower Planting.
## AIM:
To write a Java program to for given constraints.
You are given n gardens, labelled from 1 to n.

You also have a list called paths, where each element paths[i] = [xi, yi] represents a bidirectional road connectingthe  garden xi and garden yi.

You want to plant one flower in each garden, and there are exactly 4 types of flowers labelled as 1, 2, 3, and 4.

Your goal is to plant flowers such that:

No two connected gardens (i.e., connected via a path) have the same flower type.

Return any valid flower assignment as an array where:

answer[i] is the flower type planted in the (i+1) ᵗʰ garden

It is guaranteed that:

No garden is connected to more than 3 other gardens

A valid flower assignment always exists

<img width="177" height="292" alt="image" src="https://github.com/user-attachments/assets/36aa40cb-1cdd-4746-b1a6-fc51ce6e96aa" />

## Algorithm

1. **Input:**

   * Read the number of gardens `n` and the number of paths `m`.
   * Read each path pair `(u, v)` that connects two gardens.

2. **Graph Construction:**

   * Create an adjacency list for all gardens.
   * For every path `(u, v)`, add each garden to the other's adjacency list.

3. **Initialization:**

   * Create an integer array `flowers[n]` to store the flower type (1–4) assigned to each garden.
   * Each garden can have one of four flower types.

4. **Assignment Logic:**

   * For each garden `i`, check all adjacent gardens.
   * Mark the flower types already used by its neighbors and assign the first available flower type to garden `i`.

5. **Output:**

   * Print the final flower type assigned to each garden in order.
  

## Program:
```
/*
Program to implement Reverse a String
Developed by: Vignesh M
Register Number: 212223240176
*/
import java.util.*;

public class GardenFlowerPlanner {

    public static int[] assignFlowers(int n, int[][] paths) {
        @SuppressWarnings("unchecked")
        List<Integer>[] adj = new ArrayList[n];
        for (int i = 0; i < n; i++) adj[i] = new ArrayList<>();

        for (int[] path : paths) {
            adj[path[0] - 1].add(path[1] - 1);
            adj[path[1] - 1].add(path[0] - 1);
        }

        int[] flowers = new int[n];
        for (int i = 0; i < n; i++) {
            boolean[] used = new boolean[5];
            for (int nei : adj[i]) {
                used[flowers[nei]] = true;
            }
            for (int f = 1; f <= 4; f++) {
                if (!used[f]) {
                    flowers[i] = f;
                    break;
                }
            }
        }
        return flowers;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt(); 
        int m = sc.nextInt(); 

        int[][] paths = new int[m][2];
        for (int i = 0; i < m; i++) {
            paths[i][0] = sc.nextInt();
            paths[i][1] = sc.nextInt();
        }
        int[] result = assignFlowers(n, paths);

        for (int flower : result) {
            System.out.print(flower + " ");
        }
        System.out.println();
    }
}

```

## Output:
<img width="454" height="426" alt="image" src="https://github.com/user-attachments/assets/22e872d3-bb54-4586-8c01-ffe010576fe5" />



## Result:
The program successfully implemented and the expected output is verified.




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
