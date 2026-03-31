

## Overview
This project implements an optimal power flow (OPF) analysis of a 9-bus test system using MATLAB. The system is modeled in dq-coordinates, transforming voltages, currents, and network equations into the direct-quadrature domain for efficient numerical solution.

---

## Objective
- Formulate power flow equations in dq-domain  
- Solve power flow using constrained optimization (`fmincon`)  
- Validate results against standard 9-bus system data  

---

## System Model

### 9-Bus Network
![9-Bus Network](01_9bus_network_diagram.png)

- Standard 9-bus benchmark system used for analysis  

---

### Line Connectivity Data
![Line Connectivity](02_line_connection_table.png)

- Defines how buses are interconnected  
- Used to construct the node incidence matrix  

---

## Mathematical Formulation

### dq-Domain Representation (M-Matrix)
![M-Matrix](03_m_matrix_representation.png)

- Relates branch currents, bus currents, and bus voltages  
- Uses d and q components instead of complex phasors  
- Enables compact and efficient system representation  

---

## Methodology

- Transform system equations into dq-domain  
- Construct network matrices (incidence and system matrices)  
- Define power flow equations as constraints  
- Solve using MATLAB `fmincon`  
- Compute voltages, angles, currents, and losses  

---

## Code Implementation (Key Sections)

### System Initialization

```matlab
nN = 9; % Number of nodes
nB = 9; % Number of branches
Sb = 100;
Defines system size
Sets base power for per-unit calculations
Line Data Definition
linedata = [
    1 4 0 0.0576 0 250;
    2 7 0 0.0625 0 250;
    ...
];
Defines network topology
Includes:
From bus
To bus
Line impedance
Line rating
Incidence Matrix
Gamma = [
    1 0 0 -1 0 0 0 0 0;
    ...
];
Represents connections between buses and branches
Used to build system equations
M-Matrix Construction
M = horzcat(vertcat(aux1, aux2), aux3);
Combines all system equations into one matrix
Includes:
Branch equations
Node equations
Shunt elements
Optimization Solver
[SOL, fval, exitflag] = fmincon(@myfun, X, A, b, Aeq, beq, lb, ub, @mycon, options);
Solves nonlinear constrained power flow problem
Uses:
Equality constraints (Aeq, beq)
Variable bounds (lb, ub)
Objective Function
function C = myfun(X)
    C = 1;
end
Constant objective function
Problem is solved purely based on constraints (feasibility problem)
Power Flow Constraints
FBT(r,1) = vd(2)*iNd(2) + vq(2)*iNq(2) - P(2);
Represents active power balance at a bus
Ensures:
Generated power = consumed power

Additional constraints enforce:

Reactive power balance
Voltage magnitude constraints
Slack, PV, and PQ bus conditions
Voltage Calculation
V = sqrt(vd.^2 + vq.^2);
del = (180/pi) * atan(vq ./ vd);
Computes voltage magnitude
Computes voltage angle (degrees)
Power Loss Calculation
Losses = sum(SB, 'all');
Calculates total system losses
Based on branch power flows
Results
Voltage Comparison

Comparison between actual and simulated results
Accuracy:
Voltage magnitude error < 0.01 pu
Voltage angle error < 1°
Key Insights
dq-domain modeling provides an effective alternative to traditional methods
Optimization using fmincon converges reliably
Results closely match benchmark system data
Method can be extended to:
Larger systems
Optimal power flow studies
Additional constraints
Tools Used
MATLAB
Files
dq_Optimal_Power_Flow_9Bus_fmincon.m
Conclusion

This project demonstrates the successful implementation of power flow analysis using dq-domain modeling and constrained optimization. The strong agreement with standard system data validates the approach and highlights its potential for advanced power system analysis.

Author

Royalty Holyworth Chihava
