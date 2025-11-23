
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
