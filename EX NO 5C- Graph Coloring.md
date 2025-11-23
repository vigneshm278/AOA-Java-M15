
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
