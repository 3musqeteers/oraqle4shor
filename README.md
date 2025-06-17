# oraqle4shor

This notebook includes a function called modular_multiplier that receives as its input two positive integers $a$ and $N$ with $a < N$. The function returns a controlled QuantumGate that computes $\ket{ay \mod N}$ for a given input $\ket{y}$.

### Usage:

```python
# Computing 3 times 12 modulo 15
multiplier = modular_multiplier(a=3, N=15)
num_total_qubits = multiplier.num_qubits
circuit = QuantumCircuit(num_total_qubits)

## set control input to 1
circuit.x(0)
## set y to 4
circuit.x(3) # y = 4
circuit.x(4) # y = 12

circuit.append(multiplier, range(num_total_qubits))
circuit.draw()
 
state = Statevector.from_instruction(circuit)
state_probabilities_dict = state.probabilities_dict()
max_probability_argument = max(state_probabilities_dict, key=state_probabilities_dict.get)
int(max_probability_argument[-1-ceil(log2(15)):-1], 2)
```

### Gate input

* First qubit: Control
* Next $\left\lceil\log_2(N)\right\rceil$ qubits: $\ket{y}$
* Next $\left\lceil\log_2(N)\right\rceil+2$ qubits: $\ket{0}$ - used for internal computation

### Remarks

If a and N are coprime then we reset the utility $\left\lceil\log_2(N)\right\rceil$ qubits to zero using a clever trick. Otherwise we can not apply this clever trick because a does not have an inverse modulo N.

### Reference

The implementation is based on [Stephane Beauregard. 2003. Circuit for Shor's algorithm using 2n+3 qubits. Quantum Info. Comput. 3, 2 (March 2003), 175–185](https://dl.acm.org/doi/10.5555/2011517.2011525). See also the [arXiv version](https://arxiv.org/abs/quant-ph/0205095).
