# Optimal Power Flow of a 9-Bus System using dq Modelling

## Overview
This project implements an optimal power flow (OPF) analysis of a 9-bus test system using MATLAB. The system is modelled in dq-coordinates, transforming voltages, currents, and network equations into the direct-quadrature domain for efficient numerical solution.

---

## Objective
- Formulate power flow equations in dq-domain  
- Solve power flow using constrained optimization (`fmincon`)  
- Validate results against standard 9-bus system data  

---

## System Model

### 9-Bus Network
![9-Bus Network](9bus_network_diagram.png)

Standard 9-bus benchmark system used for analysis.

---

### Line Connectivity Data
![Line Connectivity](line_connection_table.png)

Defines how buses are interconnected and is used to construct the node incidence matrix.

---

## Mathematical Formulation

### dq-Domain Representation (M-Matrix)
![M-Matrix](m_matrix_representation.png)

The constructed M-matrix has dimensions of **90 × 144**, representing the relationship between system variables in dq-coordinates.

- The total number of unknown variables in the system is **144**, meaning additional equations are required to obtain a unique solution.  
- Since the M-matrix provides **90 equations**, an additional **54 equations** are needed.

These additional equations are obtained as follows:

- **18 equations** are derived from bus conditions:  
  - Each of the **9 buses** contributes **2 equations** (based on PQ, PV, and slack bus specifications).  
  - These are implemented in the MATLAB code as **nonlinear equality constraints** within the optimization problem.

- **36 equations** are obtained by setting the voltages of all FACTS devices to zero:  
  - Both **series and shunt FACTS devices** are excluded in this case  
  - This simplifies the system by removing their influence  

The complete system is then solved using the `fmincon` function, which determines the unknown variables while satisfying all constraints.
---

## Methodology
- Transform system equations into dq-domain  
- Construct network matrices  
- Apply power flow constraints  
- Solve using MATLAB `fmincon`  
- Compute voltages, angles, and losses  

---

## Code Implementation (Summary)

The MATLAB implementation follows these key steps:

### System Initialization  
`nN = 9`, `nB = 9`, `Sb = 100`  
Defines the number of buses, branches, and base power.

---

### Line Data Definition  
`linedata = [1 4 0 0.0576 0 250; ... ];`  

Defines:
- From and to buses  
- Line impedance  
- Line ratings  

---

### Network Topology  
`Gamma = [...];`  

Represents the incidence matrix describing how buses are connected.

---

### System Matrix Construction  
`M = horzcat(vertcat(aux1, aux2), aux3);`  

Combines all system equations into a single matrix describing the network.

---

### Optimization Solver  
`[SOL, fval, exitflag] = fmincon(...)`  

Solves the power flow problem using constrained nonlinear optimization.

---

### Objective Function  
`C = 1`  

A constant objective function is used, meaning the solution is driven entirely by constraints.

---

### Power Flow Constraints  
Example:  
`vd(2)*iNd(2) + vq(2)*iNq(2) - P(2) = 0`  

Ensures power balance at each bus:
- Active power  
- Reactive power  
- Voltage constraints  

---

### Voltage Calculation  
`V = sqrt(vd.^2 + vq.^2)`  
`δ = atan(vq./vd)`  

Computes voltage magnitude and angle from dq components.

---

### Loss Calculation  
`Losses = sum(SB)`  

Calculates total system losses.

---

## Results

### Voltage Comparison
![Voltage Results](voltage_comparison_results.png)

- Voltage magnitude error < 0.01 pu  
- Voltage angle error < 1°  
- Strong agreement with benchmark system  

---

## Key Insights
- dq-domain modeling is effective for power flow analysis  
- Optimization using `fmincon` converges reliably  
- Results closely match standard 9-bus data  
- Method can be extended to:
  - Larger systems  
  - Optimal power flow problems  
  - Additional constraints  

---

## Tools Used
- MATLAB  

---

## Files
- dq_PowerFlow_without_FACTS_fmincon_9BUS.m  

---

## Conclusion
This project demonstrates the successful implementation of power flow analysis using dq-domain modeling and constrained optimization. The results validate the approach and highlight its potential for advanced power system applications.

---

## Author
Royalty Holyworth Chihava
