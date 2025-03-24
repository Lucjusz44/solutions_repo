# Problem 1
# 🔌 Equivalent Resistance Using Graph Theory

## 🌟 Why This Matters (Simple Explanation)

Calculating total resistance in complex circuits can be like solving a puzzle. Graph theory gives us superpowers to:
- **Break down complicated circuits** step by step
- **Spot hidden patterns** in resistor networks
- **Automate calculations** for computer analysis

This approach is used in:
- Circuit design software
- Power grid analysis
- Microchip layout optimization

## 📚 Core Concepts Made Simple

### 🔋 Circuit ↔ Graph Conversion
- **Nodes** = Connection points (where wires meet)
- **Edges** = Resistors (with resistance values as weights)

### 🔍 Two Key Rules
1. **Series Reduction**:
   ```math
   R_{eq} = R_1 + R_2 + ... + R_n
   ```
   (Resistors in a straight line)

2. **Parallel Reduction**:
   ```math
   \frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + ... + \frac{1}{R_n}
   ```
   (Resistors sharing both ends)

## 💻 Python Implementation

```python
import networkx as nx

def equivalent_resistance(G, start, end):
    """
    Calculate equivalent resistance between two nodes in a resistor network
    
    Parameters:
        G (nx.Graph): Graph where edges have 'resistance' attribute
        start: Starting node
        end: Ending node
    
    Returns:
        float: Equivalent resistance
    """
    # Make a copy to avoid modifying original graph
    G = G.copy()
    
    while True:
        # Check if we've simplified to a single edge
        if G.number_of_edges() == 1 and G.has_edge(start, end):
            return G[start][end]['resistance']
        
        # --- Series Reduction ---
        simplified = False
        
        # Find nodes with exactly two edges (potential series)
        for node in list(G.nodes()):
            if node not in [start, end] and len(list(G.neighbors(node))) == 2:
                neighbors = list(G.neighbors(node))
                R1 = G[node][neighbors[0]]['resistance']
                R2 = G[node][neighbors[1]]['resistance']
                
                # Remove the middle node and add series combination
                G.remove_node(node)
                G.add_edge(neighbors[0], neighbors[1], resistance=R1 + R2)
                simplified = True
                break
                
        if simplified:
            continue
            
        # --- Parallel Reduction ---
        # Find all pairs of nodes with multiple edges
        for u, v in list(G.edges()):
            if G.number_of_edges(u, v) > 1:
                # Combine all parallel edges
                parallel_resistors = [G[u][v][key]['resistance'] 
                                   for key in G[u][v]]
                Req = 1 / sum(1/R for R in parallel_resistors)
                
                # Remove all edges and add single equivalent
                G.remove_edges_from(list(G.edges(u, v)))
                G.add_edge(u, v, resistance=Req)
                simplified = True
                break
                
        if not simplified:
            break
            
    # If we exit without finding direct connection, use nodal analysis
    A = nx.adjacency_matrix(G, weight='conductance').todense()
    # Add conductance (1/R) terms for nodal analysis
    # (Implementation omitted for brevity)
    return nx.resistance_distance(G, start, end)

# Example Usage:
G = nx.Graph()
G.add_edge('A', 'B', resistance=2)
G.add_edge('B', 'C', resistance=4)
G.add_edge('C', 'D', resistance=6)
G.add_edge('A', 'D', resistance=8)

print(f"Equivalent resistance A-D: {equivalent_resistance(G, 'A', 'D'):.2f} ohms")
```

## 📊 How It Works - Step by Step

1. **Input**: A graph where:
   - Nodes = Circuit junctions
   - Edges = Resistors (with resistance values)

2. **Series Detection**:
   ```mermaid
   graph LR
       A -- R1 --> B -- R2 --> C
       Becomes:
       A -- R1+R2 --> C
   ```

3. **Parallel Detection**:
   ```mermaid
   graph LR
       A -- R1 --> B
       A -- R2 --> B
       Becomes:
       A -- (R1∥R2) --> B
   ```

4. **Repeat Until Simplified**:
   - Keep applying rules until only start and end nodes remain
   - Fall back to nodal analysis if needed

## 🔍 Real-World Examples

### Example 1: Simple Series
```python
G = nx.Graph()
G.add_edge('A', 'B', resistance=2)
G.add_edge('B', 'C', resistance=3)
# A -- 2Ω -- B -- 3Ω -- C
# Expected: 2 + 3 = 5Ω
```

### Example 2: Simple Parallel
```python
G.add_edge('A', 'B', resistance=4)
G.add_edge('A', 'B', resistance=4)
# Two 4Ω resistors in parallel
# Expected: (1/4 + 1/4)^-1 = 2Ω
```

### Example 3: Complex Network
```python
G.add_edges_from([
    ('A','B', {'resistance': 1}),
    ('B','C', {'resistance': 2}),
    ('C','D', {'resistance': 3}),
    ('A','D', {'resistance': 4}),
    ('B','D', {'resistance': 5})
])
# Combination of series and parallel paths
```

## ⚡ Efficiency Analysis

**Time Complexity**:
- Best case (simple series/parallel): O(n)
- Worst case (complex network): O(n³) for nodal analysis

**Optimization Opportunities**:
1. **Priority Reduction**: Target largest subgraphs first
2. **Caching**: Store intermediate results
3. **Hybrid Approach**: Combine with matrix methods

## 🛠 Practical Applications

1. **Circuit Design**: Quickly evaluate different layouts
2. **Fault Detection**: Identify unexpected resistances
3. **Educational Tools**: Visualize circuit simplification
4. **Power Systems**: Analyze grid impedance

## 🎓 Key Takeaways

1. **Graph Theory is Powerful**: Turns circuits into solvable math problems
2. **Two Rules Rule Them All**: Series and parallel cover most cases
3. **Automation Friendly**: Perfect for computer implementation
4. **Real-World Relevant**: Used in everything from microchips to power grids

Try modifying the code to handle:
- Circuits with capacitors/inductors
- Temperature-dependent resistors
- Three-phase power systems

