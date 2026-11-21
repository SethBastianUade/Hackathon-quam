# QuantumJam 2026 — Disociación de H₄ con VQE

Solución al desafío de [QuantumJam 2026](https://quantumjam.com.ar/challenge): calcular con VQE la curva de energía de una cadena lineal de cuatro hidrógenos (base STO-3G), obtener $R_e$ y $D_e$, y ganarle a CCSD en la zona estirada ($d \geq 1{,}8$ Å).

## Contenido

- [`quantumjam_h4_vqe.ipynb`](quantumjam_h4_vqe.ipynb): el notebook completo, con salidas incluidas. Corre de punta a punta en Google Colab e instala sus propias dependencias.

## Método

1. **PySCF:** Hartree-Fock, CCSD y FCI para 15 distancias entre 0,6 y 3,0 Å, más las integrales moleculares.
2. **Jordan-Wigner** implementado a mano: 8 espín-orbitales → 8 qubits, 185 términos de Pauli. Se verifica contra FCI.
3. **Ansatz UCCSD filtrado por simetría** ($D_{\infty h}$): 26 → 14 excitaciones por capa, casi la mitad de compuertas CZ. Se comparan 1 y 2 capas.
4. **Parámetros:** optimización sin ruido (BFGS) con *warm start* entre distancias.
5. **Energía reportada:** medida con `SamplerV2` de `qiskit_aer`. Los términos se agrupan por conmutación qubit a qubit (46 grupos), los shots se reparten de forma óptima según una corrida piloto, y σ sale de la varianza muestral. Los circuitos se transpilan a `cz, sx, x, rz` con qubits conectados en línea.
6. **Análisis:** $R_e$ y $D_e$ con bootstrap, diagnósticos de confiabilidad (varianza de la energía, $\langle S^2\rangle$, convergencia en capas) y tabla final con energía, σ, shots y CZ por punto.

## Abrir en Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SethBastianUade/Hackathon-quam/blob/main/quantumjam_h4_vqe.ipynb)

La ejecución completa tarda alrededor de 20 minutos.
