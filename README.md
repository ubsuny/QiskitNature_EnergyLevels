# QiskitNature_EnergyLevels
## Description
+ This is a midterm project for computational physics 1. In this project, I will **use Qiskit nature to study electronic structure and calculate ground state**.

+ There are three main parts in the project: **Literature**, **implementation** and **presentation**. The jupyter notebooks for literature and implementation part can be found in the corresponding folder. The presentation part includes this README file and a **[YouTube-style video]()**.

+ Note that **this project is mainly based on tutorials from [Qiskit Nature Documentation](https://qiskit.org/documentation/nature/index.html)**. The References can be found at the corresponding jupyter notebook and the license can be found in the main folder.

## Literature

### Introduction

As a new topic in the course of computaional physics this semester, quantum computer provides novel and fantastic ways for us to study and solve problems that we used to solve in classical way and surely shows the possibility that we could use it to solve complicate problems more effectively. One simple example we have seen is the half adder circuit that can calculate 1+1=2 by **IBM Qiskit**, but could **we extend our vision to more complex problem that also related to the physics**? The answer is definitely yes. The amazing platform that we will use in this midterm project is the **Qiskit Nature**, which is designed to bridge the gap between natural sciences and quantum simulations. For this midterm project, **we will only focus on the simple part of solution-driven ones by applying them to study the physical problem of energy level and trying to understand and rewrite the codes from given examples.**

Let me briefly talk about the **physical context of this midterm project:** As we know for classical particles, they can have any amount of energy or the so-called "continuous energy" and can be solved by using Lagrangian mechanics or Hamiltonian mechanics from the classical theory. As a contrast, things become different when we dive in the micorscopic world of quantum mechanics since particles in quantum system are bound or spatially confined, which means they can only take on certain discrete values of energy and they are called **energy levels** as shown on the picture below. The energy spectrum of a system with such discrete energy levels is said to be **quantized** and such terms are commonly used for the energy levels of the electrons in atoms, ions, or molecules, which are bound by the electric field of the nucleus[[1]](https://en.wikipedia.org/wiki/Energy_level).The **Electronic configurations**, which is the distribution of electrons of an atom or molecule in atomic or molecular orbitals, describe each electron as moving independently in an orbital in an average field created by all other orbitals [[2]](https://en.wikipedia.org/wiki/Electron_configuration). Then the energy associated to an electron is that of its orbital and the energy of a configuration is often approximated as the sum of the energy of each electron, neglecting the electron-electron interactions. The configuration that corresponds to the lowest electronic energy is called the **ground state**, also known as the zero-point energy of the system [[3]](https://en.wikipedia.org/wiki/Ground_state). Any other configuration with energy greater than the ground state is an excited state and electrons are able to move from one configuration to another by the emission or absorption of a quantum of energy, in the form of a photon.

<img src="energy levels.png" alt="energy levels" height="20%" width="20%"/>

However, for examples form the **Qiskit Nature documentation** [[4]](https://qiskit.org/documentation/nature/index.html) that we will study, they prefered the definition of **electronic structure** in quantum chemistry, which is refered as **the state of motion of electrons in an electrostatic field created by stationary nuclei** [[5]](https://en.wikipedia.org/wiki/Electronic_structure). Except for a small number of simple problems such as hydrogen-like atoms, the solution of electronic structure problems require modern computers and such problems are routinely solved with quantum chemistry computer programs. In fact, electronic structure calculations rank among the most computationally intensive tasks in all scientific calculations, thus for this reason, quantum chemistry calculations take up significant shares on many scientific supercomputer facilities.

### Reasons to use Qiskit Nature

In the new release of Qiskit Nature[[6]](https://research.ibm.com/blog/qiskit-application-modules), the added problem-based structure enables a straightforward extension of Qiskit Nature toward other problem classes in condensed matter, lattice field theories, and biology. As part of this release, Qiskit Nature would also allow us to use the ubiquitous second quantization formalism to solve a variety of problems, ranging from fundamental particle physics to solid state physics and statistical mechanics. At the same time, we can re-frame our problems by using operators that fulfill fermionic, bosonic or spin statistics.Combinations of degrees of freedom subject to different commutation rules are also supported, thus **in short these functionalities will enable the implementation of arbitrary Hamiltonians, making Qiskit Nature a nice toolbox to solve physical problems in quantum approach**. However, to compute and slove problems that are used to be dealed with in classical way from a quantum computer, one would still need some assiantce from classcial computer. In other word, **there is no such method to build a "complete quantum computer" which has no similarity with a classical one at least for Qiskit Nature**. Qiskit Nature is not only powerful to make it easy and convenient for us to define our problem in the way of quantum computer, but **can also helps map it onto the quantum computer once we have defined our problem**. Second quantized operators are mapped onto qubit operators that can be directly used in quantum computations, without losing the information related to the nature of the problem.

### Comparision with other method

In this part, I would also like to **introduce briefly another method to solve electronic structure problems so that we can compare that with our quantum method**, Qiskit Nature, to see the advantages and disadvantages that quantum computer brings with. The comparing method I chose is by using machine learning to solve electronic structure problems from a paper in recent year[[7]](https://www.nature.com/articles/s41524-019-0162-7).

The starting point of this paper is that simulations based on solving the Kohn-Sham (KS) equation of density functional theory (DFT) have become a vital component of chemical sciences research but despite its versatility, routine DFT calculations are usually limited to a few hundred atoms due to the computational bottleneck posed by the KS equation. Thus authors of this paper came up with the idea to **introduce a machine-learning-based scheme** to efficiently **assimilate the function of the KS equation** and by-pass it to directly, rapidly, and accurately **predict the electronic structure of a material or a molecule, given just its atomic configuration**. The figure below shows their main ideas and the schematic of the hierarchical-paradigm of applying surrogate models to different outputs of first-principles calculations.

<img src="comparision fig.1.png" alt="comparision fig.1" height="60%" width="60%"/>

Then their main approach to achieve this purpose is to **utilize a new rotationally invariant representation to map the atomic environment** around a grid-point to the electron density and local density of states at that grid-point and **such mapping is learned by using a neural network** trained on previously generated reference DFT results at millions of grid-points. The figure below shows their main steps and the overview of the process used to generate surrogate models for the charge density and density of states.

<img src="comparision fig.2.png" alt="comparision fig.2" height="60%" width="60%"/>

In summary, the **advantage** for this method **is that the proposed paradigm allows for the high-fidelity emulation of KS DFT and orders of magnitude are faster than the direct solution**. But its **disadvantage is that the machine learning prediction scheme is strictly linear-scaling with system size**.

## Implementation

### The Hartree-Fock initial state

The platform we use in this project, Qiskit Nature, is interfaced with different classical codes, which are able to find the HF solutions. Interfacing between Qiskit Nature and the following codes is already available: "Gaussian", "Psi4", "PyQuante" and "PySCF". In the following I will set up a PySCF driver for the hydrogen molecule at equilibrium bond length (0.735 angstrom) in the singlet state and with no charge.

```python
#We have to first install the linting tools in Jupyter notebook
!pip install flake8 pycodestyle_magic
#To enable automatic linting use cell magic:
%load_ext pycodestyle_magic
%pycodestyle_on
from qiskit_nature.drivers import UnitsType, Molecule
from qiskit_nature.drivers.second_quantization import ElectronicStructureDriverType, ElectronicStructureMoleculeDriver
molecule = Molecule(geometry=[['H', [0., 0., 0.]],
                              ['H', [0., 0., 0.735]]], charge=0, multiplicity=1)
driver = ElectronicStructureMoleculeDriver(molecule, basis='sto3g', driver_type=ElectronicStructureDriverType.PYSCF)
```

### The mapping from fermions to qubits

The Hamiltonian we have for HF method is expressed in terms of fermionic operators, and here we will map these operators to spin operators so that we can encode the problem into the state of a quantum computer. Here we set up the Electronic Structure Problem to generate the Second quantized operator and a qubit converter that will map it to a qubit operator.

```python
from qiskit_nature.problems.second_quantization import ElectronicStructureProblem
from qiskit_nature.converters.second_quantization import QubitConverter
from qiskit_nature.mappers.second_quantization import JordanWignerMapper, ParityMapper
es_problem = ElectronicStructureProblem(driver)
second_q_op = es_problem.second_q_ops()
print(second_q_op[0])
```

If we now transform this Hamiltonian for the given driver, we get our qubit operator:

```python
qubit_converter = QubitConverter(mapper=JordanWignerMapper())
qubit_op = qubit_converter.convert(second_q_op[0])
print(qubit_op)
```

Note that in the minimal (STO-3G) basis set 4 qubits are required. We can reduce the number of qubits by using the Parity mapping, which allows for the removal of 2 qubits by exploiting known symmetries arising from the mapping.

```python
qubit_converter = QubitConverter(mapper=ParityMapper(), two_qubit_reduction=True)
qubit_op = qubit_converter.convert(second_q_op[0], num_particles=es_problem.num_particles)
print(qubit_op)
```

Now that the Hamiltonian is ready, it can be used in a quantum algorithm to find information about the electronic structure of the corresponding molecule. 

### The ground state solver

Since we alreday defined the molecular system, we then need to define a solver, which is the algorithm through which the ground state is computed. Let’s first start with a purely classical example: the NumPy minimum eigensolver. This algorithm exactly diagonalizes the Hamiltonian. Although it scales badly, it can be used on small systems to check the results of the quantum algorithms.

```python
from qiskit.algorithms import NumPyMinimumEigensolver
numpy_solver = NumPyMinimumEigensolver()
```

To find the ground state we could also use the Variational Quantum Eigensolver (VQE) algorithm, which works by exchanging information between a classical and a quantum computer as depicted in the following figure.

<img src="vqe.png" alt="comparision fig.2" height="70%" width="70%"/>

Then Let’s initialize a VQE solver like following:

```python
from qiskit.providers.aer import StatevectorSimulator
from qiskit import Aer
from qiskit.utils import QuantumInstance
from qiskit_nature.algorithms import VQEUCCFactory
quantum_instance = QuantumInstance(backend=Aer.get_backend('aer_simulator_statevector'))
vqe_solver = VQEUCCFactory(quantum_instance)
```

One could also use any available ansatz / initial state or even define one’s own. For instance:

```python
from qiskit.algorithms import VQE
from qiskit.circuit.library import TwoLocal
tl_circuit = TwoLocal(rotation_blocks=['h', 'rx'], entanglement_blocks='cz',
                      entanglement='full', reps=2, parameter_prefix='y')
another_solver = VQE(ansatz=tl_circuit,
                     quantum_instance=QuantumInstance(Aer.get_backend('aer_simulator_statevector')))
```

## Results

## Outlook

## References

[[1] https://en.wikipedia.org/wiki/Energy_level](https://en.wikipedia.org/wiki/Energy_level);

[[2] https://en.wikipedia.org/wiki/Electron_configuration](https://en.wikipedia.org/wiki/Electron_configuration);

[[3] https://en.wikipedia.org/wiki/Ground_state](https://en.wikipedia.org/wiki/Ground_state);

[[4] https://qiskit.org/documentation/nature/index.html](https://qiskit.org/documentation/nature/index.html);

[[5] https://en.wikipedia.org/wiki/Electronic_structure](https://en.wikipedia.org/wiki/Electronic_structure);

[[6] https://research.ibm.com/blog/qiskit-application-modules](https://research.ibm.com/blog/qiskit-application-modules);

[[7] https://www.nature.com/articles/s41524-019-0162-7](https://www.nature.com/articles/s41524-019-0162-7);

## License

This project uses the [Apache License 2.0](https://github.com/Qiskit/qiskit-nature/blob/main/LICENSE.txt).
