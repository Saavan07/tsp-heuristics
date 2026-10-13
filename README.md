# TSP Heuristics

## Overview
This project implements a linked-list–based solution to the **Traveling Salesperson Problem (TSP)** as part of my Data Structures course. The TSP asks: given *N* points on a plane, how can a salesperson visit all of them (and return home) while minimizing total travel distance? Finding the optimal solution is intractable for large *N*, but heuristics provide practical approximations.

Our implementation supports two greedy heuristics:
- **Nearest Neighbor Insertion**: insert each new point after the point in the tour it is closest to.
- **Smallest Increase Insertion**: insert each new point where it causes the least increase in total tour distance.

The heuristics compare tour quality against insertion in the original input order; neither guarantees an optimal tour.

---

## Project Structure
- **`Point.java`**
  Represents a 2D point (x, y). Provides distance calculations and string formatting.

- **`TourInterface.java`**
  Defines the core operations every `Tour` must support: `add`, `print`, `draw`, `distance`, `insertNearest`, and `insertSmallest`.

- **`Tour.java`**
  Implements a linked list of `Point`s with a closing tour edge using an inner `ListNode` class. Provides:
  - `add(Point p)`: append point to the end of the tour.
  - `print()`: print all points in order.
  - `draw(Graphics g)`: draw the tour on a `DrawingPanel`.
  - `distance()`: compute total tour length (including last → first).
  - `insertNearest(Point p)`: greedy nearest neighbor insertion.
  - `insertSmallest(Point p)`: greedy smallest increase insertion (O(N²)).

- **`TSPClientDriver.java`**
  Entry point of the program. Reads a dataset, constructs tours with different heuristics, times them, and visualizes results in `DrawingPanel`.

- **`DrawingPanel.java`**
  Utility class (provided) for rendering points and tours graphically.

- **`data/`**
  Contains point set files (e.g., `tsp5.txt`, `tsp1000.txt`) for testing.

- **`doc/Traveling Salesperson.pdf`**
  Original assignment specification and background information.

---

## Usage
### Compile
```bash
javac -d out src/*.java
java -cp out TSPClientDriver
```
By default the driver runs on data/tsp1000.txt. To use another supplied dataset, edit:
```
private static final String file = "data/tsp1000.txt";
```
in TSPClientDriver.java, then recompile.

---

## Sample Results
On the 5-point dataset (`tsp5.txt`):
- **Add order**: 741.58
- **Insert Nearest**: 577.79
- **Insert Smallest**: 467.79

On the 1,000-point dataset (`tsp1000.txt`):
- **Add order**: 327,693.76
- **Insert Nearest**: 27,868.71
- **Insert Smallest**: 17,265.63

These distances were reproduced from the checked-in implementation and datasets on October 13, 2026. The GUI was compiled but not interactively verified during this audit.

---

## Algorithm Analysis
- **Add order**: O(N) to build the full tour with constant-time appends; tour quality depends on input order.
- **Insert Nearest**: O(N²) : for each point, scan the current tour to find the nearest neighbor.
- **Insert Smallest**: O(N²) : for each point, test insertion between every pair of nodes to find the minimal delta.

The table above was reproduced with OpenJDK 17 on October 13, 2026 using the included datasets. Runtime depends on the machine and input; no timing guarantee is made.

---

## Visualization
Running the driver spawns three windows:
1. Original order tour
2. Nearest neighbor tour
3. Smallest increase tour

Each displays:
- Points as small circles
- Tour edges as connecting lines
- Tour statistics (size, distance, runtime)

---

## Acknowledgments
- Assignment spec adapted from Robert Sedgewick & Kevin Wayne (Princeton) and Baker Franke (UChicago).
- `DrawingPanel` utility provided with the assignment.
