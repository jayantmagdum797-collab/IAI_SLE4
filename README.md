# IAI_SLE4
# ROUTE-FINDING SYSTEM (BFS / DFS)
## SLE-4 - Architecture Decision Record (ADR)
================================================================

Course   : 02AML204 - Introduction to Artificial Intelligence
Name     : Jayant Uttam Magdum
PRN      : 25UAM135
Division : B
Date     : 8 October 2026



1. ABOUT THE PROJECT
----------------------------------------------------------------
This project is a Route-Finding System. A road network is modelled
as a graph: intersections are nodes and roads are edges. Given a
start point and a destination, the system returns the sequence of
intersections to travel through.

Two uninformed search strategies are implemented and compared:

  - Breadth-First Search (BFS): FIFO queue as the Frontier
  - Depth-First Search (DFS): stack as the Frontier

Both use an Explored Set (no intersection is expanded twice) and
share the same neighbour lookup (get_neighbors), so the comparison
is fair. Both return (path_length, nodes_expanded).


2. DECISION (ADR SUMMARY)
----------------------------------------------------------------
Decision : Use BFS with an Explored Set as the main route finder.
           DFS is kept only as a comparison baseline.
Status   : Accepted

Alternatives considered:
  - DFS with an Explored Set: low memory, but routes are not
    optimal and depend on road order.
  - Dijkstra / Uniform-Cost Search: shortest route in km, but needs
    a priority queue and road lengths.
  - A* with straight-line-distance heuristic: expands far fewer
    nodes, but the heuristic module is not built yet.

The full ADR is in the Word file:
  SLE4_25UAM116_Madhur-Bhandari_RouteFinding.docx



4. HOW TO RUN
----------------------------------------------------------------
Requirements: Python 3.8 or later. No extra libraries are needed.

  python route_finding.py

The program prints the results and saves them in results.json.
The map is generated with a fixed random seed (116), so the map,
node counts and path lengths are the same on every run. Timings
change slightly from run to run.


5. SAMPLE MAP USED FOR TESTING
----------------------------------------------------------------
  Intersections : 225 (15 x 15 grid)
  Roads         : 344 (some removed at random, 12 diagonal
                  "highways" added, lengths 0.5 to 3.0 km)
  Reachable     : 222 intersections from the start corner
  Start point   : top-left corner (intersection 0)

Test cases (destinations at 4, 12 and 21 roads from the start).


6. RESULTS
----------------------------------------------------------------
Test case        Algo  Time (ms)  Nodes   Roads  Distance (km)
---------------  ----  ---------  -----   -----  -------------
Short route      BFS     0.0061      11       4        7.4
Short route      DFS     0.0832     181     121      212.6
Medium route     BFS     0.0262      80      12       17.4
Medium route     DFS     0.0492     193     133      235.3
Long route       BFS     0.0573     213      21       36.7
Long route       DFS     0.0343      80      38       65.4

Dijkstra (shortest distance) for the long route: 29.1 km.

Key findings:
  - BFS always returned the route with the fewest roads.
  - BFS expanded about 1.5x fewer nodes on average (101 vs 151).
  - DFS was faster on the long route only by luck, and its route
    was 81% longer (38 roads vs 21).
  - BFS minimises the number of roads, not the distance. On the
    long route it was 26% longer in km than Dijkstra's route.

NOTE: These results come from a sample map, not real city data.


7. LIMITATIONS AND NEXT STEPS
----------------------------------------------------------------
  - BFS ignores road lengths, so it may not give the shortest
    route in km.
  - BFS memory grows as O(b^d), which is too much for a real
    city-scale map.
  - Next step: add a Heuristic Module and move to Dijkstra / A*.


8. CONTRIBUTION
----------------------------------------------------------------
AI tool used: Claude (Anthropic). It helped adapt the ADR layout,
write the benchmark script and format the Word file. The decision
and its justification are explained and defended by me in the viva.
