# 1. Routing

Routing is the process of deciding which path network traffic should take to reach its destination efficiently, minimizing end-to-end transmission time.

## Key Concepts

- **Autonomous System (AS):**  
  A collection of networks under a single administrative entity (e.g., an ISP, university, or tech company) using a common routing policy.

- **Routing Policy:**  
  Defines which IP ranges the AS controls and how it communicates connectivity with neighboring systems.

---

## Protocol Categories

### 1. Interior Gateway Protocols (IGP)
Handle routing **within a single AS**.

**Examples:**
- RIP (Routing Information Protocol)
- OSPF (Open Shortest Path First)
- IS-IS

---

### 2. Exterior Gateway Protocols (EGP)
Handle routing **between different ASs**.

**Example:**
- BGP (Border Gateway Protocol) — standard protocol of the global internet

---

## 2. The Routing Table

Every router maintains a routing table in its memory to determine where to send incoming packets.

### Standard Routing Table Structure:
| Destination Network | Interface (Next Hop) | Metric (Cost) |
| :--- | :--- | :--- |
| The target IP range (CIDR notation) | The outgoing port or next router address | A value representing the "expense" of the path |

### Metric Variations:
* **RIP:** Uses "Hop Count" (number of routers to pass through).
* **OSPF:** Uses connection speed (bandwidth).
* **IS-IS:** Uses a default cost of 10 per link (manually adjustable).

---

## 3. Distance Vector Routing (Bellman-Ford Algorithm)

Distance vector protocols are based on the **Bellman-Ford algorithm**. In these protocols, routers only share information with their immediate neighbors.

### The Bellman-Ford Equation
The shortest path from node **x** to node **y** is calculated as:
 * d_x(y) = \min_v \{ c(x,v) + d_v(y) \}
* c(x,v): Cost of the immediate link from x to neighbor v.
* d_v(y): Neighbor v's best known cost to reach y.

### Relaxation Process
Relaxation is the iterative step of updating the known cost to a destination if a cheaper path is found through a neighbor.

### Step-by-Step Execution:
1.  **Initialization:** A router knows only the cost to its direct neighbors. All other paths are set to infinity.
2.  **Sharing:** Every 30 seconds (in RIP), routers send a "Distance Vector" (a digest of their best known paths) to neighbors.
3.  **Update:** Upon receiving a neighbor's vector, the router applies the Bellman-Ford equation to update its own table.
4.  **Convergence:** The process repeats until all routers have the shortest paths (typically requiring V-1 iterations, where V is the number of vertices).

* **Count to Infinity Problem:** If a link fails, routers may bounce incorrect "cheap" paths back and forth, slowly incrementing the cost to infinity.
* **Slow Convergence:** Information takes time to propagate across the network as it moves node-by-node.

---

## 4. Link State Routing (Dijkstra’s Algorithm)

Link state protocols allow routers to have a complete map of the entire Autonomous System.

1.  **Link State Advertisements (LSA):** Routers flood the network with information about their local links (not their whole table).
2.  **Link State Database (LSDB):** Every router stores all LSAs in a synchronized database.
3.  **Dijkstra’s Algorithm:** Each router independently runs Dijkstra's algorithm on its LSDB to find the shortest path tree.


### Step-by-Step Dijkstra:
1.  Mark the starting node distance as 0 and all others as .
2.  Select the unvisited node with the smallest distance (initially the start node).
3.  For the current node, consider all unvisited neighbors and calculate their tentative distances.
4.  Compare the new tentative distance to the current assigned value and assign the smaller one.
5.  Once all neighbors are considered, mark the current node as visited. A visited node will not be checked again.
6.  Repeat until the destination is marked visited or all nodes are checked.

### Comparison:
| Feature | Distance Vector (RIP) | Link State (OSPF / IS-IS) |
| :--- | :--- | :--- |
| **View of Network** | Only neighbors | Entire topology map |
| **Algorithm** | Bellman-Ford | Dijkstra |
| **Updates** | Periodic (Full table) | Triggered/Incremental (LSA) |
| **Convergence** | Slow | Fast |
| **CPU/Memory** | Low | High |

---

## 5. Scaling Large Networks: OSPF Areas

To prevent the computational cost ((V^3)) from overwhelming routers in massive networks, OSPF uses **Areas**.

* **Internal Routers:** Only need the topology for their specific area.
* **Area Border Routers (ABR):** Connect different areas and summarize routing information.
* **Backbone Area (Area 0):** All other areas must connect to the backbone to pass traffic between them.

This hierarchy keeps the Link State Database (LSDB) small and prevents a single link failure from triggering a re-calculation across the entire global network.


