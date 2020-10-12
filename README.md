# Lyah-Language
Instructions for how to use the terminal on elyah.io 

# lyah
Lyah language specifies the quantum instructions for Elyah.io. 

## Documentation

### Declarations
|Gate  |Description  |Example|
|--|--|--|
|LET q[n] |Declare a qubit register of size n | LET q[5] |

### Quantum gates
|Gate  |Description  |Example|
|--|--|--|
| X q[k] | Apply the  X gate (NOT) to qubit k. | X q[1]  |
| Y q[k]  | Apply the  Y gate  to qubit k. |X q[1]  |
| Z q[k]  | Apply the  Z gate  to qubit k. |Z q[1]  |
| H q[k]  | Apply the  Hadamard gate  to qubit k. |H q[1] |
| S q[k]  | A rotation of angle -π/2  around the Z axis.  |S q[1] |
| SDG q[k]  | A rotation of angle π/2  around the Z axis.  |SDG q[1] |
| T q[k]  | A rotation of angle π/4  around the Z axis.  |T q[1] |
| TDG q[k]  | A rotation of angle -π/4  around the Z axis.  |T q[1] |
| ID q[k]  | The identity operation.  |ID q[1] |
| C q[k]  | Introduces a control in the qubit k. This control affects the gates in current step.  |ID q[1] |
| A q[k]  | Introduces an anti-control in the qubit k. This control affects the gates in current step.   |ID q[1] |

These gates support ranges of qubits 
|  |  |
|--|--|
|X q[0:4] | X gate on qubits from 0 to 3 |
|X q[:3] | X gate on qubits from 0 to 2 |
|X q[3:] | X gate on qubits from 3 to the end   |
|X q[:] | X gate on all qubits in q |
|C q[1,2,8] | Control gates on qubits 1,2,8 |

### Quantum gates with arguments
The angles of these quantum gates can be any mathematical expression and builtin functions "sqrt","sin", "cos","tan","pow","ln","abs".   

|Gate  |Description  |Example|
|--|--|--|
| RX (α) q[i]  | Rotate on axis-x with angle α   | RX (pi) q[0] |
| RY (α) q[i]  | Rotate on axis-y with angle α   | RY (pi) q[0] |
| RZ (α) q[i]  | Rotate on axis-z with angle α   | RZ (pi) q[0] |
| U1 (α) q[i]  | U1 gate with angle α   | U1 (pi) q[0] |
| U2 (α,β) q[i]  |  U2 gate with angle α,β  | U2 (pi,pi + 3*pi,pi/4) q[0] |
| U3 (α,β,γ) q[i]  | U3 gate with angle α,β,γ  | U3 (pi,sin(pi),pow(2,3)) q[0] |

These gates support ranges of qubits 
|  |  |
|--|--|
|RZ (α) q[0:4] | RZ (α) gate on qubits from 0 to 3 |
|RZ (α) q[:3] | RZ (α) gate on qubits from 0 to 2 |
|RZ (α) q[3:] | RZ (α) gate on qubits from 3 to the end   |
|RZ (α) q[:] | RZ (α) gate on all qubits in q |
|RZ (α) q[1,2,8] | RZ (α) gate on qubits 1,2,8 |


### SWAP gates
|Gate  |Description  |Example|
| SWAP q[i] q[j]  | Swap the qubits i and j  | SWAP q[0] q[2]; |
Currently, there can be only one SWAP gate in the current step.

Example:
```
LET q[4]

X q[0];
H q[1]
SWAP q[0] q[2]
X q[2];

```
In the second step, the SWAP gate clear all other gates in the step. 

### Controlled gates
|Gate  |Description  |Example|
|--|--|--|
| CX q[x]>q[z]  | Adds control on qubit x, X-gate on z | CX q[0] > q[5] |
| CX q[x:y]>q[z]  | Adds control on qubits x, to y-1, X-gate on z | CX q[:5] > q[5] |
| AX q[x:y]>q[z]  | Adds anti-Control on qubits x, to y-1, X-gate on z | AX q[2:5] > q[5] |
   

### Logical gates
(Next update)
|Gate   |Description   |Example |
|-- |-- |-- |
| AND q[i] q[j] > q[k] | Applies toffoli. Control on qubits i and j, and X gate on target qubit k.   | AND q[1] q[2] > q[3]   |

| NOT q[k] | The same as X gate   | NOT q[1,2,3]   |


### MACROS 
|Gate   |Description   |Example |
|-- |-- |-- |
| HADALL  | Hadamard all declared qubits in the current step  | HADALL  |
| INIT  | The same as above  | INIT  |
| XALL  | X gate all declared qubits in the current step  | XALL  |
| CXALL  | Control on all qubits in the current step, X on last   | CXALL  |
| AXALL  | Anti-Control on all qubits in the current step, X on last   | AXALL  |

## Label definition (Next update)
|Gate   |Description   |Example |
|-- |-- |-- |
|DEF q[k] label | Labels qubit k as label | DEF q[1] ancilla |
|DEF q[x:y] label | Labels qubits x to y-1  as label | DEF q[1:100] grover|
 
Works for  gates ID,C,A,X,Y,Z,H,S,SDG,T,TDG,NOT. 

Example 
```
LET q[4]
DEF q[0:2] grover

X grover;
H grover
```


## Examples
![picture 1](/pictures/cir1.png)  
The next lyah source code define the circuit above. Each step or column of the circuit si separated by semicolon ';' 
```
LET q[4]

X q[0]
X q[1];
C q[0]
C q[1]
A q[2]
X q[3];
```

