# Quantum Teleportation

This project is a Qiskit simulation of quantum teleportation, a protocol that moves a qubit's exact state from one place to another without measuring it, copying it, or physically sending the qubit. It runs entirely on a simulator, so no quantum hardware is needed.

## The problem

Alice has a qubit in an unknown state, and she wants Bob to end up with a qubit in exactly the same state. She can't simply measure it and tell him the result, because measuring destroys the state and gives her only a single 0 or 1. She also can't make a copy, because quantum states can't be copied. Teleportation solves this using one entangled pair that Alice and Bob share in advance, plus two ordinary bits that Alice sends to Bob.

## How it works

Alice starts with her mystery qubit and her half of the entangled pair, while Bob holds the other half. She applies a CNOT from the mystery qubit to her half of the pair, then an H gate to the mystery qubit, which spreads the state across all three qubits. She then measures her two qubits and sends the two resulting bits to Bob over a normal channel.

At this point, Bob's qubit already holds the mystery state, but scrambled in one of four ways depending on Alice's results. If the second bit is 1, his qubit's 0 and 1 are swapped, so he applies an X gate. If the first bit is 1, the sign of its 1 part is flipped, so he applies a Z gate. After these corrections, his qubit is in exactly the state Alice started with.

## Checking the result

The amplitudes of a qubit can't be read directly, so the notebook checks the result in two ways. First, it compares how often Bob's qubit gives 0 and 1 with the original state, which confirms that the probabilities match. Second, it applies the steps that created the mystery state in reverse to Bob's qubit. If the state arrived exactly, this turns it back into ket 0, and measuring it gives 0 every time.

As a control, the notebook also runs the protocol without Bob's corrections, as if Alice's two bits never arrived. In that case, the reverse check produces a large share of 1s, which shows that the classical bits are essential.

## Results

Each check is measured 1,000 times. With Bob's corrections, the reverse check gives 0 in all 1,000 runs, which confirms the state was teleported exactly. Without them, roughly half the results are 1, so the state arrives scrambled. The mystery state is chosen at random each run, so the exact numbers vary.

## Run it

```bash
pip install qiskit qiskit-aer matplotlib pylatexenc jupyter
jupyter notebook teleportation.ipynb
```

## Notes

The state is moved rather than copied, since Alice's original is destroyed when she measures it. Teleportation also can't send anything faster than light, because Bob's qubit is useless until Alice's two bits reach him through a normal channel. Those two bits are completely random, so anyone who intercepts them learns nothing about the state itself. Teleportation is a key building block for quantum networks and for linking separate quantum processors together.

Made with [Qiskit](https://www.ibm.com/quantum/qiskit).
