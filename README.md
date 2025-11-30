Dijikstra

DijkstraShortestPath
  
    import java.util.*;
    public class DijkstraShortestPath {

    static final int INF = Integer.MAX_VALUE;

    static void dijkstra(int n, List<List<int[]>> graph, int src) {
        int[] dist = new int[n];
        int[] parent = new int[n];
        Arrays.fill(dist, INF);
        Arrays.fill(parent, -1);

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));

        dist[src] = 0;
        pq.add(new int[]{0, src});

        while (!pq.isEmpty()) {
            int[] top = pq.poll();
            int u = top[1];

            for (int[] edge : graph.get(u)) {
                int v = edge[0];
                int weight = edge[1];

                if (dist[v] > dist[u] + weight) {
                    dist[v] = dist[u] + weight;
                    parent[v] = u;
                    pq.add(new int[]{dist[v], v});
                }
            }
        }

        System.out.println("\nNode\tDistance from Source " + src + "\tPath");
        for (int i = 0; i < n; i++) {
            System.out.print(i + "\t\t");
            if (dist[i] == INF) {
                System.out.println("∞\t\t\tNo path");
                continue;
            }

            System.out.print(dist[i] + "\t\t\t");

            List<Integer> path = new ArrayList<>();
            for (int v = i; v != -1; v = parent[v])
                path.add(v);

            for (int j = path.size() - 1; j >= 0; j--) {
                System.out.print(path.get(j));
                if (j != 0) System.out.print(" -> ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of nodes: ");
        int n = sc.nextInt();

        System.out.print("Enter number of edges: ");
        int m = sc.nextInt();

        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++)
            graph.add(new ArrayList<>());

        System.out.println("Enter edges in the format: from to weight");
        for (int i = 0; i < m; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            int w = sc.nextInt();
            graph.get(u).add(new int[]{v, w});
        }

        System.out.print("Enter source node (0 to " + (n - 1) + "): ");
        int source = sc.nextInt();

        dijkstra(n, graph, source);

        sc.close();
    }
    }


Compile the Program
In VS Code terminal, go to the folder where the file is saved.
type-
javac DijkstraShortestPath.java

Run the Program-
java DijkstraShortestPath

.
.
.
.
.
.

OSPF STYLE PROGRAM

    import java.util.*;

    public class OSPFLinkStateRouting {

    static final int INF = Integer.MAX_VALUE;

    // Dijkstra's algorithm: computes shortest path tree from source
    static void dijkstra(int n, List<List<int[]>> graph,
                         int src, int[] dist, int[] parent) {

        Arrays.fill(dist, INF);
        Arrays.fill(parent, -1);

        // Min-priority queue: (distance, node)
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));

        dist[src] = 0;
        pq.add(new int[]{0, src});

        while (!pq.isEmpty()) {
            int[] top = pq.poll();
            int u = top[1];

            for (int[] edge : graph.get(u)) {
                int v = edge[0];
                int w = edge[1];

                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    parent[v] = u;
                    pq.add(new int[]{dist[v], v});
                }
            }
        }
    }

    // Helper: find next hop from src to dest using parent[]
    static int getNextHop(int src, int dest, int[] parent) {
        if (src == dest) return -1;  // No next hop for self

        int v = dest;
        int p = parent[v];

        // Move backwards until we reach the node just after src
        while (p != -1 && p != src) {
            v = p;
            p = parent[v];
        }

        if (p == -1) return -1;  // Not reachable
        return v;                // First hop after src
    }

    // Print OSPF-style routing table
    static void printOSPFRoutingTable(int n, int src,
                                      int[] dist, int[] parent) {

        System.out.println("\nOSPF-style Routing Table for Router (Node) " + src + ":");
        System.out.printf("%-12s%-12s%-12s%n", "Destination", "Cost", "Next Hop");

        for (int i = 0; i < n; i++) {
            System.out.printf("%-12d", i);

            if (dist[i] == INF) {
                System.out.printf("%-12s%-12s%n", "INF", "-");
                continue;
            }

            System.out.printf("%-12d", dist[i]);

            int nh = getNextHop(src, i, parent);
            if (nh == -1)
                System.out.printf("%-12s%n", "-");
            else
                System.out.printf("%-12d%n", nh);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of nodes: ");
        int n = sc.nextInt();

        System.out.print("Enter number of edges: ");
        int m = sc.nextInt();

        // Adjacency list: graph[u] = list of {v, weight}
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        System.out.println("Enter edges in the format: from to weight");
        for (int i = 0; i < m; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            int w = sc.nextInt();
            // Undirected network (bidirectional links)
            graph.get(u).add(new int[]{v, w});
            graph.get(v).add(new int[]{u, w});
        }

        System.out.print("Enter source router (0 to " + (n - 1) + "): ");
        int src = sc.nextInt();

        int[] dist = new int[n];
        int[] parent = new int[n];

        // Link State Routing: run Dijkstra from this router
        dijkstra(n, graph, src, dist, parent);

        // Generate OSPF-style routing table
        printOSPFRoutingTable(n, src, dist, parent);

        sc.close();
    }
    }

Compile using- 
javac OSPFLinkStateRouting.java

Run using- java OSPFLinkStateRouting

.
.
.
.
.
.
.
.
