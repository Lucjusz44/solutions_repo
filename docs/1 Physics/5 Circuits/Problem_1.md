# Problem 1
# ⚡ **Equivalent Resistance Made Stupid Simple** ⚡  
*(No physics jargon, just straight-up clarity with emoji power!)*  

---

## **🎯 Why Bother With This?**  
- 🔌 **Circuits get messy** – too many resistors = headache.  
- 🧠 **Graph theory** turns chaos into order (like magic).  
- 💻 **Computers love this method** – great for automation.  

---

## **🛠️ Tools You Need**  
1. **Graph Theory Basics**  
   - 🟢 **Nodes** = Connection points (where wires meet).  
   - 🔵 **Edges** = Resistors (with resistance values).  

2. **Two Golden Rules**  
   # ⚡ **Resistance Rules for Normal People** ⚡  

### **🔗 Series (One After Another)**  
➡ **Total Resistance = Just Add Them!**  
```  
R_total = R₁ + R₂ + R₃ + ...  
```  
**Example:**  
`2 + 3 + 5 = 10Ω` *(Like stacking weights – total gets heavier!)*  

---

### **🔄 Parallel (Side by Side)**  
➡ **For TWO Resistors:**  
```  
R_total = (R₁ × R₂) / (R₁ + R₂)  
```  
**Example:**  
Two `4Ω` resistors:  
`(4 × 4) / (4 + 4) = 2Ω` *(Like two roads – traffic flows easier!)*  

➡ **For THREE+ Resistors:**  
```  
1. Multiply all: R₁ × R₂ × R₃  
2. Divide by sum of pairs: (R₁R₂ + R₁R₃ + R₂R₃)  
```  
**Example:**  
Three `2Ω` resistors:  
`(2×2×2) / (4 + 4 + 4) = 8/12 ≈ 0.67Ω`  

*(Yes, parallel reduces resistance – more paths = easier flow!)*  

---

### **💡 Pro Tips:**  
✅ **Series:** More resistors = Higher total resistance  
✅ **Parallel:** More resistors = Lower total resistance  
✅ **Always convert parallel to two resistors first if possible!**  


---

## **🔍 Step-by-Step Simplification**  

### **1️⃣ Find Resistors in Series (Straight-Line Gang)**  
- **Look for:** Resistors connected **end-to-end** with **no splits**.  
- **Action:** Replace them with **one big resistor** (sum them up).  

**Example:**  
- `R₁ = 2Ω` + `R₂ = 3Ω` → **Total = 5Ω** ✅  

### **2️⃣ Find Resistors in Parallel (Side-by-Side Squad)**  
- **Look for:** Resistors **sharing the same start & end points**.  
- **Action:** Use the **parallel formula** to merge them.  

**Example:**  
- `R₁ = 4Ω` || `R₂ = 4Ω` → **Total = 2Ω** ✅  

### **3️⃣ Repeat Until Only One Resistor Remains**  
- Keep simplifying **series & parallel** until you get **one final R**.  

---

## **💻 Let’s Code It! (Python Example)**  
*(For those who want automation!)*  

```python
import networkx as nx

def simplify_circuit(G):
    while True:
        # 1. Check if we're done (only 1 resistor left)
        if G.number_of_edges() == 1:
            return list(G.edges(data=True))[0][2]['resistance']
        
        # 2. Try simplifying series resistors
        simplified = False
        for node in list(G.nodes()):
            neighbors = list(G.neighbors(node))
            if len(neighbors) == 2:  # Series candidate
                R1 = G[node][neighbors[0]]['resistance']
                R2 = G[node][neighbors[1]]['resistance']
                G.remove_node(node)
                G.add_edge(neighbors[0], neighbors[1], resistance=R1 + R2)
                simplified = True
                break
        
        if simplified:
            continue
        
        # 3. Try simplifying parallel resistors
        for u, v in list(G.edges()):
            if G.number_of_edges(u, v) > 1:  # Parallel resistors
                total_R = 1 / sum(1 / G[u][v][k]['resistance'] for k in G[u][v])
                G.remove_edges_from(list(G.edges(u, v)))
                G.add_edge(u, v, resistance=total_R)
                simplified = True
                break
        
        if not simplified:
            break  # Can't simplify further
    
    return "Complex circuit! Need advanced methods."

# Example usage
G = nx.Graph()
G.add_edge('A', 'B', resistance=2)
G.add_edge('B', 'C', resistance=3)
G.add_edge('A', 'C', resistance=6)

print("Total resistance:", simplify_circuit(G))
```

---

## **📊 Real-World Examples**  

### **1️⃣ Simple Series Circuit**  
- `A --[2Ω]-- B --[3Ω]-- C`  
- **Total = 2 + 3 = 5Ω**  

### **2️⃣ Simple Parallel Circuit**  
- `A --[4Ω]-- B`  
- `A --[4Ω]-- B`  
- **Total = 2Ω**  

### **3️⃣ Mixed Circuit**  
- `A --[2Ω]-- B --[3Ω]-- C`  
- `A --[6Ω]-- C`  
- **Total = 4Ω** (after simplification)  



