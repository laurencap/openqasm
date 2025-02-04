# OpenQASM


OpenQASM is an imperative programming language for describing quantum circuits. It is capable of
describing universal quantum computing using the circuit model, measurement-based model,
and near-term quantum computing experiments.

This repo contains the OpenQASM specification, examples, and tools for the OpenQASM intermediate representation.

OpenQASM is a [Qiskit project](https://qiskit.org).

## :bangbang: Call for TSC Applicants! :bangbang:

We are currently looking for applications to join the [Technical Steering Committee](https://github.com/openqasm/openqasm/blob/main/governance.md#structures).
If you are interested in standing in the election, please contact John Lapeyre, the current Secretary, at [john.lapeyre@ibm.com](mailto:john.lapeyre@ibm.com) before the 30th of November, 2023.

The election will be held in a OpenQASM TSC meeting (open to all Contributors) at a later date, to be announced.
Watch this space, or the [\#open-qasm channel on the Qiskit Slack server](https://qiskit.slack.com/archives/CG8JSE0UB) ([invitation link](https://ibm.co/joinqiskitslack)) for further details.

## Current version: **3.0**

* Live language specification [**version 3.0**](https://openqasm.github.io/)

* The branch of this repository for the previous version: [OpenQASM 2.0](https://github.com/openqasm/openqasm/tree/OpenQASM2.x)

## About this project

In this repository, you'll find all the documentation related to OpenQASM, some useful OpenQASM examples, and [plugins for some text editors](#plugins).

### Language specs

The live [language documentation](https://openqasm.github.io/) specification.

### Examples

An example of OpenQASM 3.0 source code is given below. Several more examples may be found in the [examples folder](examples).
```text
/*
 * Repeat-until-success circuit for Rz(theta),
 * cos(theta-pi)=3/5, from Nielsen and Chuang, Chapter 4.
 */
OPENQASM 3;
include "stdgates.inc";

/*
 * Applies identity if out is 01, 10, or 11 and a Z-rotation by
 * theta + pi where cos(theta)=3/5 if out is 00.
 * The 00 outcome occurs with probability 5/8.
 */
def segment qubit[2] anc, qubit psi -> bit[2] {
  bit[2] b;
  reset anc;
  h anc;
  ccx anc[0], anc[1], psi;
  s psi;
  ccx anc[0], anc[1], psi;
  z psi;
  h anc;
  measure anc -> b;
  return b;
}

qubit input;
qubit[2] ancilla;
bit[2] flags = "11";
bit output;

reset input;
h input;

// braces are optional in this case
while(int(flags) != 0) {
  flags = segment ancilla, input;
}
rz(pi - arccos(3 / 5)) input;
h input;
output = measure input;  // should get zero
```

## Citation format

For research papers, we encourage authors to reference.

- [Version 3.0] Andrew W. Cross, Ali Javadi-Abhari, Thomas Alexander, Niel de Beaudrap, Lev S. Bishop, Steven Heidel, Colm A. Ryan, John Smolin, Jay M. Gambetta, Blake R. Johnson "OpenQASM 3: A broader and deeper quantum assembly language" [[arxiv:2104.14722]](https://arxiv.org/abs/2104.14722).
- [Previous Version: 2.0] Andrew W. Cross, Lev S. Bishop, John A. Smolin, Jay M. Gambetta "Open Quantum Assembly Language" [[arXiv:1707.03429]](https://arxiv.org/abs/1707.03429).

## Governance

The OpenQASM project has a process for accepting changes to the language and making decisions codified in its [governance model](governance.md).


## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](./LICENSE) file for details.


## Contributing

If you'd like to help please take a look to our [contribution guidelines](CONTRIBUTING.md). This project adheres to a [Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Release Notes

See the section on Release Notes [contribution guidelines](CONTRIBUTING.md#release-notes).
