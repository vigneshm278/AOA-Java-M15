
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
