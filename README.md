# QiskitNature_EnergyLevels
## Description
+ This is a midterm project for computational physics 1. In this project, I will **use Qiskit nature to study electronic structure and calculate ground state**.

+ There are three main parts in the project: **Literature**, **implementation** and **presentation**. The jupyter notebooks for literature and implementation part can be found in the corresponding folder. The presentation part includes this README file and a **[YouTube-style video]()**.

+ Note that **this project is mainly based on tutorials from [Qiskit Nature Documentation](https://qiskit.org/documentation/nature/index.html)**. The References can be found at the corresponding jupyter notebook and the license can be found in the main folder.

## Literature

### Introduction

As a new topic in the course of computaional physics this semester, quantum computer provides novel and fantastic ways for us to study and solve problems that we used to solve in classical way and surely shows the possibility that we could use it to solve complicate problems more effectively. One simple example we have seen is the half adder circuit that can calculate 1+1=2 by **IBM Qiskit**, but could **we extend our vision to more complex problem that also related to the physics**? The answer is definitely yes. The amazing platform that we will use in this midterm project is the **Qiskit Nature**, which is designed to bridge the gap between natural sciences and quantum simulations. For this midterm project, **we will only focus on the simple part of solution-driven ones by applying them to study the physical problem of energy level and trying to understand and rewrite the codes from given examples.**

Then let me briefly talk about the **physical context of this midterm project:** As we know for classical particles, they can have any amount of energy or the so-called "continuous energy" and can be solved by using Lagrangian mechanics or Hamiltonian mechanics from the classical theory. As a contrast, things become different when we dive in the micorscopic world of quantum mechanics since particles in quantum system are bound or spatially confined, which means they can only take on certain discrete values of energy and they are called **energy levels** as shown on the picture below. The energy spectrum of a system with such discrete energy levels is said to be **quantized** and such terms are commonly used for the energy levels of the electrons in atoms, ions, or molecules, which are bound by the electric field of the nucleus[[1]](https://en.wikipedia.org/wiki/Energy_level).The **Electronic configurations**, which is the distribution of electrons of an atom or molecule in atomic or molecular orbitals, describe each electron as moving independently in an orbital in an average field created by all other orbitals [[2]](https://en.wikipedia.org/wiki/Electron_configuration). Then the energy associated to an electron is that of its orbital and the energy of a configuration is often approximated as the sum of the energy of each electron, neglecting the electron-electron interactions. The configuration that corresponds to the lowest electronic energy is called the **ground state**, also known as the zero-point energy of the system [[3]](https://en.wikipedia.org/wiki/Ground_state). Any other configuration with energy greater than the ground state is an excited state and electrons are able to move from one configuration to another by the emission or absorption of a quantum of energy, in the form of a photon.

<img src="energy levels.png" alt="energy levels" height="20%" width="20%"/>

However, for examples form the **Qiskit Nature documentation** [[4]](https://qiskit.org/documentation/nature/index.html) that we will study, they prefered the definition of **electronic structure** in quantum chemistry, which is refered as **the state of motion of electrons in an electrostatic field created by stationary nuclei** [[5]](https://en.wikipedia.org/wiki/Electronic_structure). Except for a small number of simple problems such as hydrogen-like atoms, the solution of electronic structure problems require modern computers and such problems are routinely solved with quantum chemistry computer programs. In fact, electronic structure calculations rank among the most computationally intensive tasks in all scientific calculations, thus for this reason, quantum chemistry calculations take up significant shares on many scientific supercomputer facilities.

### Reasons to use Qiskit Nature

In the new release of Qiskit Nature[[6]](https://research.ibm.com/blog/qiskit-application-modules), the added problem-based structure enables a straightforward extension of Qiskit Nature toward other problem classes in condensed matter, lattice field theories, and biology. As part of this release, Qiskit Nature would also allow us to use the ubiquitous second quantization formalism to solve a variety of problems, ranging from fundamental particle physics to solid state physics and statistical mechanics. At the same time, we can re-frame our problems by using operators that fulfill fermionic, bosonic or spin statistics.Combinations of degrees of freedom subject to different commutation rules are also supported, thus **in short these functionalities will enable the implementation of arbitrary Hamiltonians, making Qiskit Nature a nice toolbox to solve physical problems in quantum approach**. However, to compute and slove problems that are used to be dealed with in classical way from a quantum computer, one would still need some assiantce from classcial computer. In other word, **there is no such method to build a "complete quantum computer" which has no similarity with a classical one at least for Qiskit Nature**. Qiskit Nature is not only powerful to make it easy and convenient for us to define our problem in the way of quantum computer, but **can also helps map it onto the quantum computer once we have defined our problem**. Second quantized operators are mapped onto qubit operators that can be directly used in quantum computations, without losing the information related to the nature of the problem.

## References

[[1] https://en.wikipedia.org/wiki/Energy_level](https://en.wikipedia.org/wiki/Energy_level);

[[2] https://en.wikipedia.org/wiki/Electron_configuration](https://en.wikipedia.org/wiki/Electron_configuration);

[[3] https://en.wikipedia.org/wiki/Ground_state](https://en.wikipedia.org/wiki/Ground_state);

[[4] https://qiskit.org/documentation/nature/index.html](https://qiskit.org/documentation/nature/index.html);

[[5] https://en.wikipedia.org/wiki/Electronic_structure](https://en.wikipedia.org/wiki/Electronic_structure);

[[6] https://research.ibm.com/blog/qiskit-application-modules](https://research.ibm.com/blog/qiskit-application-modules);

[[7] https://github.com/Qiskit/qiskit-nature](https://github.com/Qiskit/qiskit-nature);

## License

This project uses the [Apache License 2.0](https://github.com/Qiskit/qiskit-nature/blob/main/LICENSE.txt).
